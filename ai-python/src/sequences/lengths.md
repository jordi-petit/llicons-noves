# Application: Word Lengths

<img src='./lengths.png' style='height: 8em; float: right; margin: 2em 0 1em 1em;'/>

This lesson presents a program to read a poem and obtain some statistics about the length of its words.

## Statement and example

Given a poem, suppose we are interested in knowing which is the longest word, how many words it contains, and what is the average length (in number of letters).

For example, if our input was

```text
when the geese go to the field the first goes ahead
the second goes behind and then comes the third
```

we should get as output

```text
number of words: 20
average length: 4.0
longest word: second
```

If there are several words with the same maximum number of letters, any one of them can be printed.

## Solution

To solve this task, we will consider that the poem consists of a non-empty sequence of words. Therefore, we use a loop

```python
for word in tokens():
    ...
```

to process all words one after another. Fortunately, the `tokens()` function applied to strings already works for words (assuming they are separated by spaces or line breaks).

Inside the loop, we only need to keep the necessary information to provide the answers at the end of the program, once all words have been processed. For this, we will use the following variables:

- `number_words` is an integer that counts the number of words. Logically, it should be initialized to zero. At each iteration, it is incremented by one. After the loop, it will contain the total number of words in the poem.

- `sum_lengths` is an integer that accumulates the lengths of the words. It should also be initialized to zero. At each iteration, it is incremented by the length of the current word. After the loop, it will contain the total sum of letters of all the words in the poem.

- `longest_word` is a string representing the longest word found so far (the first one in case of a tie). We can initialize it with the empty string `''` because any other word will always be longer. At each iteration, we check if the current word is longer than `longest_word` and, if so, replace it with the current word. After the loop, it will contain one of the longest words in the poem (specifically, the one read first).

After the loop, we just need to report the calculated values.

The complete program is as follows:

```python
from yogi import tokens

def main():
    # initializations
    number_words = 0
    sum_lengths = 0
    longest_word = ''
    # main loop for each word in the input
    for word in tokens(str):
        number_words = number_words + 1
        sum_lengths = sum_lengths + len(word)
        if len(word) > len(longest_word):
            longest_word = word
    # print the result
    print('number of words:', number_words)
    print('average length:', sum_lengths / number_words)
    print('longest word:', longest_word)


if __name__ == '__main__':
    main()
```

It should be noted that this program only works correctly if the poem has at least one word, because otherwise the average length would not be well defined.

<Authors authors="jpetit roura"/>
