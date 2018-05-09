# Academic essay generation

The aim of the project was to collect as many student written essays as possible and to train a text generation model on them. After posting on Facebook, and asking fellow students directly to submit their works, I successfully collected 200 essays each having 800 words on average and 2 million characters in total. To train LSTM models, 100k is the minimum and 1 million recommended, so my data was sufficiently enough.

While the collection went fairly smoothly, the preprocessing had a few hicups. I ahd to deal with unstructured PDFs, unicode and special characters. I ended up doing a lot of research and experimentation with a number of approaches for text generation. All approaches were based in some way on [Andrej Karpathy's projects](http://karpathy.github.io/2015/05/21/rnn-effectiveness/). First, I experimented with a [Bidirectional LSTM model extended with a Doc2Vec similarity model that was developed by] campdav(https://github.com/campdav/Text-Generation-using-Bidirectional-LSTM-and-Doc2Vec-models). I had to extend the text preprocessing to make it more robust. The Keras model was added more LSTM, fully connected layers and an Embedding layer. Unfortunately, the system didn't didn't perform well, which further fine-tuning might have solved, but according to a few stackoverflow threads, bidirectional LSTMs are not emant to be used for text generation and training took too long, as I had to train two model every time (LSTM and Doc2Vec). As a result, I looked for a simpler project and for the final product, I ended up using [mattdangerw's keras-text-generation](https://github.com/mattdangerw/keras-text-generation) project, which a simple LSTM text generator. Again, I extended the project with the same preprocessing pipeline and replaced the constant length input sequences with singel sentences tokenized using NLTK. 

The final output of the system was published in [the NYUAD student magazine](https://www.thegazelle.org/issue/138/features/artificial-intelligence-the-future-of-essay-writing), as a satire for the way liberal arts students use fancy words without meaning to meet the word count. The project is also displayed on [an interactive website](http://gaborcsapo.com/pages/essays/).

## Components and prereqs
* Preprocessing
    - Jupyter notebook to convert and clean text from .doc and .pdf files
* Bidirectional LSTM experiment
    - inspired by [campdav's Text-Generation-using-Bidirectional-LSTM-and-Doc2Vec-models](https://github.com/campdav/Text-Generation-using-Bidirectional-LSTM-and-Doc2Vec-models) project with changes to preprocessing and the model architeture
    - Keras with Tensorflow backend
    - Spacy and NLTK for preprocessing and tokenization
    - Gensim Doc2Vec
* LSTM
    - based on [mattdangerw's keras-text-generation](https://github.com/mattdangerw/keras-text-generation) project with changes to some of the text preprocessing
    - Keras with Tensorflow
    - Colorama
    - NLTK
* Web visualization of essays
    - website typing out live some of the generated essays
    - Typeit.js
    - Bootstrap 3


## Getting started

Clone the project from Github, and install prerequisites.
Put training corpus in a single txt file in either folder's data folder and call it input.txt. 

To train the Bidirectional LSTM, follow the instructions presented in notebook 1, 2, 3. They will train the required models and generate an example sentence. Play with the temperature, as it makes  huge difference in sentence generation.

To train the simple LSTM and to generate a sentence:
```
python train.py --seq-length 100 --num-layers 6 --num-epochs 100 --live-sample 
# Sample with a random seed for 500 characters and a less random output
python sample.py --length 500 --temperature 0.7
```
To run the visualization, simply open the index.html in Chrome.

## Contributing

Please feel free to reach out to me, if you would like to continue to use the project. Unfortunately, I won't be able to release the essays that I trained on.


## Authors

* **Gabor Csapo** - gabor.csapo@nyu.edu

Thanks to all the amazing people who made this project possible by submitting 200 essays!


## Project Background


