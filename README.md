# Attenion-in-Transformers
This repository covers the concepts and details of attention mechanisms, including how to implement them.

## Transformers have three main parts:

* Word Embedding: Converts words or tokens into numerical representations.
* Positional Encoding: Keeps track of word order. For example, while "Aditya eats pizza" and "Pizza eats Aditya" contain the same words, their meanings differ due to word positions. 🍕 
* Attention: Establishes relationships among words. Self-attention works by evaluating how similar each word is to every other word in the sentence (including itself). These similarity scores help determine how the Transformer encodes each word.

## Matrix Math in Self Attention

Q = Query
K = Key
V = Value

Query, Key, and Value come from database terminology.
Query: Represents the user's input.
Key: Items in the database ranked by their similarity to the query.
Value: The item returned by a key in response to a query.



![Pasted Graphic 3](https://github.com/user-attachments/assets/4255f4c1-3156-4ccc-b4ac-e4327c1393b8)


Let's understand using an example - Prompt: Write a poem.

1. Step 1: The Transformer converts each word into a word embedding.

2. Step 2: The Transformer adds positional encoding to these word embeddings.

For example, let's represent each word with 2 numbers:

* Write: 1.16, 0.23
* A: 0.57, 1.36
* Poem: 4.41, -2.16

Note: In practice, it's common to use 512 or more numbers to represent each word.

- Creating a Query:
To create a query for each word, we:

Stack the encoded words into a matrix.
Multiply the matrix by a 2x2 matrix of query weights, resulting in 2 query numbers per word.

![poem 4 41 -2 16](https://github.com/user-attachments/assets/f6f6dc54-963d-4a6d-9081-253f6882c010)

- We multiple the encoded values by a 2x2 matrix because we used 2 encoded values for words. And a 2x2 matrix allows us to end up with 2 query numbers per word. 
- Similarly	we multiply the encoded values to a 2x2 matrix of key weights and a 2x2 matrix of value weights. 


### Step 1
- Calculate a dot product between Query Matrix Q and  Transpose Key Matrix K. (Why Transpose? To enable matrix multiplication between 2 matrices.)
- Dot products can be used as an unscaled measure of similarity between 2 things, and this metric is closely related to cosine similarity. The big difference is that cosine similarity scales the dot product to be between -1 and 1.


![-0 30](https://github.com/user-attachments/assets/dc529bf8-680a-4e44-816e-ff498f72ddee)

This means -0.09 is unscaled similarity between query and key for the word write. 
- By doing this we end up with unscaled dot product similarities between all possible combinations of Queries and Keys for each word.

### Step 2
- Divide this matrix with dimension of Key matrix. Dimension refers number of values for each token, which is 2 in our case.
- Note: Scaling by just the square root of number of values per token doesn’t scale the dot product  similarities in any kind of systematic way. 

![Unscaled Dot](https://github.com/user-attachments/assets/5d521b03-25f2-4944-9fce-6d31a79c0246)


### Step 3
- Take SoftMax of each row in the output matrix
- Softmax function makes it so that sum of each row is 1.
  
![Scaled Dot Product](https://github.com/user-attachments/assets/1dc3ad89-6619-4fd2-b200-c87dd22c79ac)

This gives us an overall summary of similarity
- Example: The word write is 36% similar to itself, 40% similar to a and 24% similar to poem.
- The percentages that come out of Softmax function tell us how much influence each word should have on final encoding for any given word. 

### Step 4
- Multiply this matrix by value matrix V to get the Self-Attention Scores
  
![Similarities](https://github.com/user-attachments/assets/b9a3252d-46be-4f47-8ed5-b88cebc46c08)










