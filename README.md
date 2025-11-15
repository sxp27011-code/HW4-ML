# HW4 700762701 SREE RAM CHARAN TEJA PUDARI
 This script builds a small text preprocessing pipeline using two popular NLP libraries: spaCy and NLTK. The goal is to clean and extract useful information from a sentence by performing tokenization, stopword removal, lemmatization, and POS (part-of-speech) filtering.

import spacy from nltk.corpus import stopwords import nltk (spaCy is used for tokenization, POS tagging, and lemmatization. NLTK provides a list of common English stopwords (e.g., “the”, “is”, “and”). The nltk module is used to download required datasets.)

nltk.download('stopwords') (NLTK requires downloading external datasets before use. This ensures the English stopword list is available.)

nlp = spacy.load("en_core_web_sm") (This loads a pre-trained English model. The model includes tokenization rules, word vectors, POS tagging, dependency parsing, and lemmatization. It is required for transforming raw text into structured tokens.)

text = "John enjoys playing football while Mary loves reading books in the library." (This is the example sentence used to demonstrate preprocessing.)

doc = nlp(text) (The text is passed into spaCy’s pipeline. The output doc contains all tokens with rich linguistic annotations: token.text → original word token.lemma_ → base form token.pos_ → part of speech token.is_alpha → alphabetic check and more)

stop_words = set(stopwords.words('english')) (Stopwords are common words that do not carry important meaning (e.g., “in”, “the”, “and”, “while”). Using set() speeds up lookup time.)

filtered = [ (token.text, token.lemma_, token.pos_) for token in doc if token.is_alpha and token.text.lower() not in stop_words and token.pos_ in ["NOUN", "VERB"] ] This is the most important part of the script. Each token must satisfy three filters:

token.is_alpha Ensures only alphabetic tokens are kept. Removes numbers, punctuation, symbols, etc.
token.text.lower() not in stop_words Removes common stopwords. Example removed words: while, in, the
token.pos_ in ["NOUN", "VERB"] Keeps only meaningful content words: NOUN → football, books, library VERB → enjoys, playing, reading, loves This helps focus on words that carry semantic meaning.
Collected Output For each remaining token, the script stores: (original word, lemma, part of speech)



Q2) This script performs two main NLP tasks using spaCy: Named Entity Recognition (NER) – detects people, organizations, locations, products, etc. Pronoun Ambiguity Detection – checks whether pronouns like he, him, she, they could cause confusion when multiple people appear in the text. Below is a step-by-step explanation of what each part of the script does.

import spacy (spaCy is the NLP library used for: tokenization Named Entity Recognition (NER) part-of-speech tagging pronoun detection)

nlp = spacy.load("en_core_web_sm") (This loads spaCy’s small English model, which contains: NER capabilities POS tagging tokenization rules The model is necessary for extracting entities and pronouns.)

text = "Chris met Alex at Apple headquarters in California. He told him about the new iPhone launch." (This sentence contains: two people (Chris, Alex) third-person pronouns (He, him) named entities (Apple, California, iPhone) This combination makes it ideal for demonstrating pronoun ambiguity.)

doc = nlp(text) (spaCy processes the text and returns a doc object containing: tokens lemmas POS tags named entities)

print("Named Entities Detected:") for ent in doc.ents: print(f" - {ent.text:25} → {ent.label_}") (This loops through all detected entities. Examples spaCy identifies: Chris → PERSON Alex → PERSON Apple → ORG California → GPE iPhone → PRODUCT The output lists each entity and its category.)

pronouns = [t.text.lower() for t in doc if t.pos_ == "PRON"] This collects all pronouns from the text using POS tags. In this case, the result is: ["he", "him"]

ambiguous_prons = {"he", "she", "they", "him", "her", "them"} if any(p in ambiguous_prons for p in pronouns): print("\n⚠️ Warning: Possible pronoun ambiguity detected!") else: print("\nNo pronoun ambiguity detected.") The script checks whether any pronoun in the sentence belongs to a set of ambiguous third-person pronouns.

Why ambiguous? If multiple people (Chris, Alex) appear in a sentence, and a pronoun like he or him is used, it’s not clear who the pronoun refers to. Since the text contains “He” and “him”, the script prints: ⚠️ Warning: Possible pronoun ambiguity detected!

print("Filtered tokens (Word – Lemma – POS):") for w, l, p in filtered: print(w, l, p) (Prints each processed token in a clean, readable format. Helps visualize how the pipeline is transforming the text.)
