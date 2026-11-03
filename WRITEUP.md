# COMP 440: Representing and Retrieving Knowledge

Claude writes your answers here as you say them. A sentence or two is enough for each. Your
Part 0 predictions stay as you first wrote them; Part 5 asks what you would change.

**Name:** XXXX
**Date:** XXXX

## Part 0: Predictions

### Questions that name a character, place or thing: which method will do better, and why?

XXXX

### Vague descriptions: which method will do better, and why?

XXXX

## Part 1: Build the TF-IDF model

### The settings you chose, and why

- REMOVE_STOP_WORDS = True: People write questions with lots of filler ("a movie where a guy..."), and those words would otherwise match nearly every overview. I'd rather the score come from content words.
- MIN_MOVIES = 1: Rare words are the whole point of TF-IDF. A name like "Punxsutawney" appears in one overview, and a higher cutoff would throw away exactly the words that identify a movie.
- REPEATS_COUNT_LESS = True: Overviews are only about 45 words, so a word said twice is probably just phrasing. I don't want one repeated word to outweigh everything else in the summary.

## Part 2: Top terms

### Your movies: what do the top terms get right and wrong about what makes each one distinctive?

XXXX

## Part 3: Your own searches

### Your questions, as you typed them

XXXX

### Did the movies you meant come up? What did TF-IDF get right and wrong?

XXXX

## Part 4: The question bank

### Which kind of question did TF-IDF handle best, and which worst? By how much?

XXXX

### The misses you looked at: what do they have in common?

XXXX

## Part 5: Embeddings

### What is a sentence embedding, in your own words?

XXXX

### Where does each method do better, and why?

XXXX

### Your Part 0 predictions: which held, and which would you change?

XXXX

### How might you combine the two methods?

XXXX
