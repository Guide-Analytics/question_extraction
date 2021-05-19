# Question Extraction

* Goals:
    * For any product, there are likely to be various inquiries that consumers have about it, some more common than other
    * These inquiries may be included in consumer reviews, particularly negative ones
    * By extractings questions from collections of reviews, it may be possible to find common inquiries
    * If common inquiries are discovered, they can be addressed through an FAQ or the like, to reduce the amount of future consumer concerns
* Overview:
    * For a given text, we split it up into sentences, and extract those that represent questions.
    * Input: A csv file with a series of review texts
    * Output: A csv file with only the questions from the input texts
* Algorithm:
    * Split text into sentences
    * For each sentence, determine whether it is a question
* Performance:
    * Low false negative rate, i.e. questions are nearly always correctly identified as such
    * Good, but slightly worse, false positive rate. A small portion of the output is typically statements that are similar to questions in some ways
    * These include certain long complex sentences, certain colloquial structures, and some highly ungrammatical sentences

## Question Determination
* Goals:
    * In order to find consumer inquiries, sentences in reviews that are questions need to be extracted
    * These may be yes or no (yn) questions or wh-questions, i.e. those that inquire about the identity of something (thing, place, reason, etc.)
    * In line with the goal of finding common inquiries, certain "indirect" questions are included
    * These consist primarily of sentence like: "I don't know why X is the case"
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
    * This is done since the wh-words in English have dual interrogative and relative meanings
    * Relative clauses are subordinate to the main clause, so they can be identified
    * However, do include some subordinate clauses that are objects of certain verbs (ex. "I don't know why...")
* Issues:
    * This is the main source of false positives
    * In many long and/or ungrammatical sentences, whether or not the wh-word is in a clause cannot be reliably determined
    * In most of these cases it causes a sentence to be falsely categorized as a question
    * Colloquial language often uses sentence fragments as full sentences, which generally makes a subordinate clause appear like the main clause, this making it appear to be a question

### YN-questions
* Algorithm:
    * Check if first word in the sentence is an auxiliary verb
    * If so, check if the following phrase is a noun phrase that is the subject of the verb
    * This is done to exclude certain imperative sentences ("Do it!") where a verb starts a sentence, but is followed by an object (if it is followed by a noun phrase)
* Issues:
    * In colloquial language subjects are sometimes dropped
    * If the main verb is an auxiliary verb, and it is followed by a noun phrase, typically the direct object, the noun phrase will be falsely identified as the subject, causing the sentence to be falsely categorized as a question