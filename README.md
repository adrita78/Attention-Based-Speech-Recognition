# Attention Based Speech Recognition

Notebooks can be run in sequential order to train the models and show example outputs (which display in jupyter) and generate metrics.

## Neural Network Architecture Summary

The neural network architecture comprises a listener feature embedder and a speller. The listener feature embedder processes input data with shape [128, 1621, 27], and the speller generates output with shape [128, 328]. The architecture is composed of a total of 3954 layers.

### Listener Feature Embedder

The listener feature embedder processes the input data and consists of convolutional layers, batch normalization, and activation functions. It takes input of shape [128, 1621, 27] and produces output of shape [128, 256, 1621].

### Speller

The speller generates the final output by processing the output from the listener feature embedder. It consists of multiple layers, including linear layers and activation functions.

## Layer Details

### Convolutional Layer (Conv1d_1)

- Input Shape: [27, 256, 5]
- Output Shape: [128, 256, 1621]
- Parameters: 34.816k
- Mult-Adds: 56.02176M

### Linear Layer (Linear_char_prob)

- Input Shape: [512, 31]
- Output Shape: [128, 31]
- Parameters: 15.903k
- Mult-Adds: 0.51118M

### Total Parameters and Mult-Adds

- Trainable parameters: 28.479775M
- Non-trainable parameters: 0.0
- Mult-Adds: 3.36951424G

## pBLSTM Architecture Summary

The pBLSTM architecture consists of a bidirectional LSTM layer followed by a truncation and reshape operation. It takes a packed sequence as input and produces a packed sequence as output.

### Bidirectional LSTM Layer

The bidirectional LSTM layer has two layers and is set to be batch-first.

## Training Details

### Epochs

Trained for close to 200 epochs in total:

- 35 epochs without scheduler or teacher-forcing
- 15 epochs with teacher forcing starting from 1.0 and reduced at 0.05 every 2 epochs
- 5 epochs with teacher forcing reduced at 0.025 rate every 2 epochs
- Additional 5 epochs with teacher forcing reduced at 0.025 rate every 2 epochs
- 20 epochs without teacher forcing

### Hyperparameters

- Optimizer: AdamW with learning rate=1e-3
- Loss Function: Cross Entropy Loss
- Teacher Forcing Rate: Varies across epochs, starting from 1 and reducing by 0.05 every 10 epochs
- Batch Size: 128

### Regularizations Used

- Locked Dropout in the pBLSTM encoder
- Weight Tying for the character embedding layer and the character probability layer in the decoder

### Data Loading Scheme

Time and Frequency Masking transforms were applied to improve the variance of the data.

### Initial Testing

The model was initially tested on a Toy Dataset to ensure proper functionality.


