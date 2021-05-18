# Question Extraction

Extracting questions from a text

* Overview:
    * For a given text, we split it up into sentences, and extract those that represent questions.
    * Input: A csv file with a series of review texts
    * Output: A csv file with only the questions from the input texts
* Algorithm:
    * Split text into sentences
    * For each sentence, determine whether it is a question
* Performance:
    * Very good at detecting questions, i.e. questions are essentially always identified as such
    * Good, but slightly worse, at detecting statements. A small portion of the output is typically statements that are similar to questions in some ways

## Question Determination
* Overview:
    * For a given sentence, we determine if it represents a question, including certain types of indirect questions.
    * Input: A sentence
    * Output: A boolean representing whether the sentence is a question
* Algorithm:
    * Questions are separated into two types: wh-questions, which are those that use a wh-word (who, what, where, etc.), and yn-questions, which expect a yes or no answer.
    * For each sentence it is determined whether it is either a wh-question or a yn-question

### Wh-questions
* Algorithm:
    * Find each instance of a wh-word
    * Make sure the wh-word is in the main clause and not a subordinate clause
    * However, do include some subordinate clauses that are objects of certain verbs (ex. "I don't know why...")
* Issues:
    * This is the main source of false positives
    * In many long and/or ungrammatical sentences, whether or not the wh-word is in a clause cannot be reliably determined
    * In most of these cases it causes a sentence to be falsely categorized as a question

### Yn-questions
* Algorithm:
    * Check if first word in the sentence is an auxiliary verb
    * If so, check if the following phrase is a noun phrase that is the subject of the verb
* Issues:
    * In colloquial language subjects are sometimes dropped
    * If the main verb is an auxiliary verb, and it is followed by a noun phrase, typically the direct object, the noun phrase will be falsely identified as the subject, causing the sentence to be falsely categorized as a question