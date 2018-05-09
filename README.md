# Academic essay generation

> "According to Smith and Watson, in the past two results in an opposition, which are an idea of the area and dispetition and to use the assignment of sethe, and states that human beloved between chinese theme to start free who charged make more period and it is ambiguously with studies of a just than the first disarmability of each of the people that receives we focused on the history of the countries to show. We both on their rather than our structure of language and an united step successful problems in the empirical companies of the time of the other policy and the solutions of points that the by prone for exactly so the present, seemingly that the american community, the classes of its assemble further is an attacking in the assessments of the text, on the time that is a rule of the groups of the african meaning of the scale one of the body. And as most previously toothbrush is several energy. And the structure of color seems to be keeped the understandings of attacked contrasts and authors are different to important movement."

- Example of generated paragraphs

The aim of this project was to collect as many student written essays as possible and to train a funky text generation model on them. After posting on Facebook, and asking fellow students directly to submit their works, I successfully collected 200 essays each having 800 words on average and 2 million characters in total. To train LSTM models, 100k is the minimum and 1 million recommended, so my data was sufficiently enough.

While the collection went fairly smoothly, the preprocessing had a few hicups. I had to deal with unstructured PDFs, unicode and special characters. I ended up doing a lot of research and experimentation with a number of approaches for text generation. All approaches were based in some way on [Andrej Karpathy's projects](http://karpathy.github.io/2015/05/21/rnn-effectiveness/). First, I experimented with a [Bidirectional LSTM model extended with a Doc2Vec similarity model that was developed by campdav](https://github.com/campdav/Text-Generation-using-Bidirectional-LSTM-and-Doc2Vec-models). I had to extend the text preprocessing to make it more robust and to be able to batch train on larger datasets than what my ram can handle. The Keras model was added more LSTM, fully connected layers and an Embedding layer. Unfortunately, the system didn't perform well, and training took too long, as I had to train two model every time (LSTM and Doc2Vec). Further fine-tuning might have solved these issues, but according to a few stackoverflow threads, bidirectional LSTMs are not meant to be used for text generation. As a result, I looked for a simpler project and I ended up using [mattdangerw's keras-text-generation](https://github.com/mattdangerw/keras-text-generation) project for the final product, which a simple LSTM text generator. Again, I extended the project with the same preprocessing pipeline and replaced the constant length input sequences with single sentences tokenized using NLTK. 

The final output of the system was published in [the NYUAD student magazine](https://www.thegazelle.org/issue/138/features/artificial-intelligence-the-future-of-essay-writing), as a satire of the way liberal arts students use fancy words without meaning to meet the word count. The project is also displayed on [an interactive website](http://gaborcsapo.com/pages/essays/).

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
Long Short Term Memory networks (LSTMs) are a special kind of Recurrent Neural Network, capable of learning long-term dependencies thanks to their special architecture that is composed of a cell, an input gate, an output gate and a forget gate. The cell is responsible for "remembering" values over arbitrary time intervals, hence the word "memory" in LSTM. Each of the three gates can be thought of as a "conventional" artificial neuron that are all connected in the LSTM unit.

Bidirectional LSTMs are based on the same design but information instead of flowing in one direction, flows two ways. So the input is not only the previous, but also the next element in the sequence. BiDLSTMs show their strength in understanding context in many tasks.

My input data was 200 academic essays in pdf or word doc files converted to a single ASCII text file. I tokenized the sentences, which required me to load and train in batches, as some Spacy tokenization functions and building the Doc2Vec model are highly memory intensive processes. 
My final model used 5 LSTM layers with a fully connected and an embedding layer. I trained for 30 epochs due to time constraint, which still took 9 hours. I would recommend training for more than 100 epochs. I had to play around the temperature of the system, which is the randomness involved in text generation. I found 0.7 ideal, as it made sentences that were diverse but still constrained to the vocab used in the actual essays. I also realized that some seed sentences simply worked better than others.
