# Streaming Transformer
**This repo contains the streaming Transformer of our work xxx. The work is based on ESPnet0.6.0. The streaming Transformer includes a streaming encoder, either chunk-based or look-ahead based, and a trigger-attention based decoder.**


## Installation
Our installation follow the installation process of ESPnet
### Step 1. setting of the environment
    CUDAROOT=/path/to/cuda
    
    export PATH=$CUDAROOT/bin:$PATH
    export LD_LIBRARY_PATH=$CUDAROOT/lib64:$LD_LIBRARY_PATH
    export CFLAGS="-I$CUDAROOT/include $CFLAGS"
    export CUDA_HOME=$CUDAROOT
    export CUDA_PATH=$CUDAROOT`
### Step 2. installation including Kaldi
    cd tools
    make -j 10
### Training

### Step 1. Data Prepare
    cd egs/librispeech/asr1
    ./run.sh 
By default. the processed data will stored in the current directory. You can change the path by editing the scripts.
### Step 2. Viterbi decoding
To train a TA based streaming Transformer, the alignments between CTC paths and transcriptions are required. In our work, we apply Viterbi decoding using the offline Transformer model.

    cd egs/librispeech/asr1
    ./viterbi_decode.sh /path/to/model


### Step 3. Train a streaming Transformer
Here, we train a chunk-based streaming Transformer which is initialized with an offline Transformer provided by ESPnet. Set `enc-init` in `conf/train_streaming_transformer.yaml` to the path of your offline model.

	cd egs/librispeech/asr1
	./train.sh

If you want to train a look-ahead based streaming Transformer, set `chunk` to False and change the `left-window, right-window, dec-left-window, dec-right-window` arguments. The training log is written in `exp/streaming_transformer/train.log`. You can monitor the output through `tail -f exp/streaming_transformer/train.log`

### Step 4. Decoding
Execute the following script with to decoding on test_clean and test_other sets

	./decode.sh num_of_gpu job_per_gpu


## Results on Librispeech
| Model        | test-clean   |  test-other  |
| --------   | -----:  | :----:  |
| streaming_transformer-chunk32      | 2.8   |       |


