# Embedding-layer-working-in-LSTM

The output vectors are not computed from the input using any mathematical operation. Instead, each input integer is used as the index to access a table that contains all posible vectors. That is the reason why you need to specify the size of the vocabulary as the first argument (so the table can be initialized).

The most common application of this layer is for text processing. Let's see a simple example. Our training set consists only of two phrases:

Hope to see you soon

Nice to see you again

So we can encode these phrases by assigning each word a unique integer number (by order of appearance in our training dataset for example). Then our phrases could be rewritten as:

[0, 1, 2, 3, 4]

[5, 1, 2, 3, 6]
Now imagine we want to train a network whose first layer is an embeding layer. In this case, we should initialize it as follows:

Embedding(7, 2, input_length=5)
The first argument (7) is the number of distinct words in the training set. The second argument (2) indicates the size of the embedding vectors. The input_length argumet, of course, determines the size of each input sequence.

Once the network has been trained, we can get the weights of the embedding layer, which in this case will be of size (7, 2) and can be thought as the table used to map integers to embedding vectors:

+------------+------------+
|   index    |  Embedding |
+------------+------------+
|     0      | [1.2, 3.1] |
|     1      | [0.1, 4.2] |
|     2      | [1.0, 3.1] |
|     3      | [0.3, 2.1] |
|     4      | [2.2, 1.4] |
|     5      | [0.7, 1.7] |
|     6      | [4.1, 2.0] |
+------------+------------+
So according to these embeddings, our second training phrase will be represented as:

[[0.7, 1.7], [0.1, 4.2], [1.0, 3.1], [0.3, 2.1], [4.1, 2.0]]
It might seem counter intuitive at first, but the underlaying automatic differentiation engines (e.g., Tensorflow or Theano) manage to optimize these vectors associated to each input integer just like any other parameter of your model.

I believe it's inaccurate to say embedding layers reduce one-hot encoding input down to fewer inputs. After all the one-hot vector is a one-dimensional data and it is indeed turned into 2 dimensions in our case. Better to be said that

embedding layer comes up with a relation of the inputs in another dimension

Whether it's in 2 dimensions or even higher.

I also find a very interesting similarity between word embedding to the Principal Component Analysis. Although the name might look complicated the concept is straightforward. What PCA does is to define a set of data based on some general rules (so-called principle components). So it's like having a data and you want to describe it but using only 2 components. Which in this sense is very similar to word embeddings. They both do the same-alike job in different context. You can find out more here. I hope maybe understanding PCA helps understanding embedding layers through analogy.

To wrap up, the answer to the original question of the post that "how does it calculate the value?" would be:

Basically, our neural network captures underlying structure of the inputs (our sentences) and puts relation between words in our vocabulary into a higher dimension (let's say 2) by optimization.
Deeper understanding would say that the frequency of each word appearing with another word from our vocabulary influences (in a very naive approach we can calculate it by hand)
Aforementioned frequency could be one of many underlying structures that NN can capture
You can find the intuition on the youtube link explaining the word embeddings.

This representation is called word embedding.

And they may look like:

I: [0.02, 0.08, 0.14, 0.16, 0.60] etc…

However, most of the time these embeddings are learnt during the bigger training task that you are already doing.

Hence, using an embedding layer which has a size equal to the vocabulary size of your dataset has become really popular.

This layer is interface between your input layer (matrix of word’s indices in the vocabulary) and the LSTM layer.

And the initialisation of this layer may be all zeros, random or pre-trained word embeddings such as GloVe, word2vec etc.

During, the forward pass of training (or test), a lookup of the words is done from this embedding layer to get a particular word embedding.

During backward pass, the gradients of the loss function also flow through the embedding layer, thus learning the word embeddings most suitable for the task at hand.

PS: A similar explanation is valid from any “vision-deep learning” task rather than “language-deep learning”.

