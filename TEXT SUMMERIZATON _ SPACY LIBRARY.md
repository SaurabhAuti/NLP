```python
#pip install spacy
#python -m spacy download en_core_web_sm (we have 3 type of model is available called small, medium & large)
```


```python
import spacy
nlp = spacy.load("en_core_web_sm")
doc = nlp("Apple is looking at buying U.K. startup for $1 billion")
```


```python
for token in doc:
    print(token.text, token.pos_)
```

    Apple PROPN
    is AUX
    looking VERB
    at ADP
    buying VERB
    U.K. PROPN
    startup NOUN
    for ADP
    $ SYM
    1 NUM
    billion NUM
    


```python
text = """There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on. The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.). The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query. Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.
An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document. Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic). This problem is called multi-document summarization. A related application is summarizing news articles. Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.
Image collection summarization is another application example of automatic summarization. It consists in selecting a representative set of images from a larger set of images.[4] A summary in this context is useful to show the most representative images of results in an image collection exploration system. Video summarization is a related domain, where the system automatically creates a trailer of a long video. This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions. Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured """
```


```python
text
```




    'There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on. The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.). The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query. Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.\nAn example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document. Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic). This problem is called multi-document summarization. A related application is summarizing news articles. Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.\nImage collection summarization is another application example of automatic summarization. It consists in selecting a representative set of images from a larger set of images.[4] A summary in this context is useful to show the most representative images of results in an image collection exploration system. Video summarization is a related domain, where the system automatically creates a trailer of a long video. This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions. Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured '




```python
import spacy
from spacy.lang.en.stop_words import STOP_WORDS
from string import punctuation
```


```python
stopwords = list(STOP_WORDS) 
stopwords
```




    ['that',
     'throughout',
     'in',
     'many',
     'less',
     'eleven',
     'thereupon',
     'itself',
     'why',
     'them',
     'him',
     'should',
     'either',
     'do',
     'from',
     'he',
     'towards',
     '’m',
     'to',
     'n’t',
     'thereafter',
     'so',
     'get',
     'another',
     'being',
     'must',
     'much',
     'namely',
     'alone',
     'three',
     'elsewhere',
     'though',
     'because',
     'hundred',
     'n‘t',
     'before',
     'doing',
     'its',
     'thru',
     'nothing',
     'latter',
     'forty',
     '’s',
     'hers',
     'none',
     'who',
     'very',
     'afterwards',
     'twelve',
     'beyond',
     'side',
     'ourselves',
     'still',
     'there',
     'about',
     'unless',
     'first',
     'hence',
     '’d',
     'indeed',
     'too',
     'seemed',
     'than',
     'such',
     'go',
     'between',
     'empty',
     'not',
     'are',
     'one',
     'it',
     'fifty',
     'while',
     'by',
     'ca',
     'eight',
     'ours',
     'become',
     'herein',
     'whereupon',
     'almost',
     'ten',
     'off',
     'around',
     'but',
     'until',
     'five',
     'just',
     'noone',
     'me',
     '’ll',
     'again',
     'last',
     'whence',
     'whole',
     'if',
     'show',
     'back',
     'done',
     'more',
     'hereby',
     'wherein',
     'someone',
     'nevertheless',
     'else',
     'as',
     'against',
     'seems',
     'how',
     'after',
     'often',
     'since',
     'those',
     'these',
     'perhaps',
     'across',
     'cannot',
     'twenty',
     'below',
     'four',
     'were',
     'whereas',
     '’ve',
     'themselves',
     're',
     'whose',
     'others',
     'the',
     'whereby',
     'take',
     'please',
     'nowhere',
     'us',
     "'d",
     'without',
     'latterly',
     'had',
     'make',
     'hereafter',
     'due',
     'thereby',
     'now',
     'be',
     'beforehand',
     'an',
     'keep',
     'up',
     'yet',
     "'re",
     'most',
     'next',
     'along',
     'can',
     'have',
     'anyway',
     'when',
     "n't",
     "'ll",
     'sometimes',
     'what',
     '‘ve',
     'thus',
     'front',
     'own',
     'into',
     'within',
     'serious',
     'his',
     'does',
     'amount',
     'regarding',
     'therefore',
     'even',
     'of',
     'part',
     'least',
     'see',
     'mostly',
     'whatever',
     'our',
     'fifteen',
     'and',
     'among',
     'third',
     'name',
     'further',
     'every',
     'above',
     'any',
     'otherwise',
     'quite',
     'same',
     'over',
     'i',
     'formerly',
     'say',
     'all',
     'move',
     'onto',
     'their',
     'thence',
     'you',
     'sometime',
     'they',
     'becomes',
     'former',
     'everyone',
     'therein',
     'per',
     'during',
     'made',
     'myself',
     'nine',
     'then',
     'moreover',
     'amongst',
     'may',
     'nobody',
     'each',
     'six',
     'also',
     'been',
     'here',
     'seem',
     'together',
     'everything',
     'at',
     'her',
     'besides',
     'using',
     'down',
     'is',
     'yours',
     'somehow',
     'once',
     'anyone',
     'bottom',
     'behind',
     'always',
     'give',
     '‘m',
     'mine',
     'out',
     'various',
     'few',
     'anything',
     'anywhere',
     'on',
     'two',
     '‘d',
     'enough',
     'anyhow',
     'will',
     'nor',
     'himself',
     'yourselves',
     'becoming',
     'whom',
     'your',
     'where',
     'whoever',
     'she',
     'am',
     'full',
     '‘ll',
     'upon',
     '‘re',
     'several',
     'we',
     'for',
     'used',
     'although',
     'some',
     'both',
     "'s",
     'however',
     'other',
     'would',
     'well',
     'never',
     "'m",
     'really',
     'top',
     'or',
     'neither',
     'seeming',
     'a',
     'everywhere',
     'put',
     'meanwhile',
     'could',
     'ever',
     'somewhere',
     'whither',
     'yourself',
     'whenever',
     'became',
     'with',
     'no',
     'toward',
     'call',
     'through',
     'sixty',
     'whether',
     'might',
     'only',
     'wherever',
     'which',
     'hereupon',
     'whereafter',
     'under',
     "'ve",
     '‘s',
     'already',
     'something',
     'rather',
     'except',
     'herself',
     'has',
     'did',
     'my',
     'this',
     'was',
     'via',
     'beside',
     '’re']




```python
len(stopwords)
```




    326




```python
nlp = spacy.load('en_core_web_sm') 
```


```python
text
```




    'There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on. The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.). The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query. Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.\nAn example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document. Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic). This problem is called multi-document summarization. A related application is summarizing news articles. Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.\nImage collection summarization is another application example of automatic summarization. It consists in selecting a representative set of images from a larger set of images.[4] A summary in this context is useful to show the most representative images of results in an image collection exploration system. Video summarization is a related domain, where the system automatically creates a trailer of a long video. This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions. Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured '




```python
doc = nlp(text)
doc
```




    There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on. The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.). The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query. Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.
    An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document. Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic). This problem is called multi-document summarization. A related application is summarizing news articles. Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.
    Image collection summarization is another application example of automatic summarization. It consists in selecting a representative set of images from a larger set of images.[4] A summary in this context is useful to show the most representative images of results in an image collection exploration system. Video summarization is a related domain, where the system automatically creates a trailer of a long video. This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions. Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured 




```python
# lets get the tokens from text
tokens = [token.text for token in doc]
print(tokens) 
#when we execute everythihg we created tokens from the text & not removed any of the stopwords & didnt cleaned the data
```

    ['There', 'are', 'broadly', 'two', 'types', 'of', 'extractive', 'summarization', 'tasks', 'depending', 'on', 'what', 'the', 'summarization', 'program', 'focuses', 'on', '.', 'The', 'first', 'is', 'generic', 'summarization', ',', 'which', 'focuses', 'on', 'obtaining', 'a', 'generic', 'summary', 'or', 'abstract', 'of', 'the', 'collection', '(', 'whether', 'documents', ',', 'or', 'sets', 'of', 'images', ',', 'or', 'videos', ',', 'news', 'stories', 'etc', '.', ')', '.', 'The', 'second', 'is', 'query', 'relevant', 'summarization', ',', 'sometimes', 'called', 'query', '-', 'based', 'summarization', ',', 'which', 'summarizes', 'objects', 'specific', 'to', 'a', 'query', '.', 'Summarization', 'systems', 'are', 'able', 'to', 'create', 'both', 'query', 'relevant', 'text', 'summaries', 'and', 'generic', 'machine', '-', 'generated', 'summaries', 'depending', 'on', 'what', 'the', 'user', 'needs', '.', '\n', 'An', 'example', 'of', 'a', 'summarization', 'problem', 'is', 'document', 'summarization', ',', 'which', 'attempts', 'to', 'automatically', 'produce', 'an', 'abstract', 'from', 'a', 'given', 'document', '.', 'Sometimes', 'one', 'might', 'be', 'interested', 'in', 'generating', 'a', 'summary', 'from', 'a', 'single', 'source', 'document', ',', 'while', 'others', 'can', 'use', 'multiple', 'source', 'documents', '(', 'for', 'example', ',', 'a', 'cluster', 'of', 'articles', 'on', 'the', 'same', 'topic', ')', '.', 'This', 'problem', 'is', 'called', 'multi', '-', 'document', 'summarization', '.', 'A', 'related', 'application', 'is', 'summarizing', 'news', 'articles', '.', 'Imagine', 'a', 'system', ',', 'which', 'automatically', 'pulls', 'together', 'news', 'articles', 'on', 'a', 'given', 'topic', '(', 'from', 'the', 'web', ')', ',', 'and', 'concisely', 'represents', 'the', 'latest', 'news', 'as', 'a', 'summary', '.', '\n', 'Image', 'collection', 'summarization', 'is', 'another', 'application', 'example', 'of', 'automatic', 'summarization', '.', 'It', 'consists', 'in', 'selecting', 'a', 'representative', 'set', 'of', 'images', 'from', 'a', 'larger', 'set', 'of', 'images.[4', ']', 'A', 'summary', 'in', 'this', 'context', 'is', 'useful', 'to', 'show', 'the', 'most', 'representative', 'images', 'of', 'results', 'in', 'an', 'image', 'collection', 'exploration', 'system', '.', 'Video', 'summarization', 'is', 'a', 'related', 'domain', ',', 'where', 'the', 'system', 'automatically', 'creates', 'a', 'trailer', 'of', 'a', 'long', 'video', '.', 'This', 'also', 'has', 'applications', 'in', 'consumer', 'or', 'personal', 'videos', ',', 'where', 'one', 'might', 'want', 'to', 'skip', 'the', 'boring', 'or', 'repetitive', 'actions', '.', 'Similarly', ',', 'in', 'surveillance', 'videos', ',', 'one', 'would', 'want', 'to', 'extract', 'important', 'and', 'suspicious', 'activity', ',', 'while', 'ignoring', 'all', 'the', 'boring', 'and', 'redundant', 'frames', 'captured']
    


```python
tokens
```




    ['There',
     'are',
     'broadly',
     'two',
     'types',
     'of',
     'extractive',
     'summarization',
     'tasks',
     'depending',
     'on',
     'what',
     'the',
     'summarization',
     'program',
     'focuses',
     'on',
     '.',
     'The',
     'first',
     'is',
     'generic',
     'summarization',
     ',',
     'which',
     'focuses',
     'on',
     'obtaining',
     'a',
     'generic',
     'summary',
     'or',
     'abstract',
     'of',
     'the',
     'collection',
     '(',
     'whether',
     'documents',
     ',',
     'or',
     'sets',
     'of',
     'images',
     ',',
     'or',
     'videos',
     ',',
     'news',
     'stories',
     'etc',
     '.',
     ')',
     '.',
     'The',
     'second',
     'is',
     'query',
     'relevant',
     'summarization',
     ',',
     'sometimes',
     'called',
     'query',
     '-',
     'based',
     'summarization',
     ',',
     'which',
     'summarizes',
     'objects',
     'specific',
     'to',
     'a',
     'query',
     '.',
     'Summarization',
     'systems',
     'are',
     'able',
     'to',
     'create',
     'both',
     'query',
     'relevant',
     'text',
     'summaries',
     'and',
     'generic',
     'machine',
     '-',
     'generated',
     'summaries',
     'depending',
     'on',
     'what',
     'the',
     'user',
     'needs',
     '.',
     '\n',
     'An',
     'example',
     'of',
     'a',
     'summarization',
     'problem',
     'is',
     'document',
     'summarization',
     ',',
     'which',
     'attempts',
     'to',
     'automatically',
     'produce',
     'an',
     'abstract',
     'from',
     'a',
     'given',
     'document',
     '.',
     'Sometimes',
     'one',
     'might',
     'be',
     'interested',
     'in',
     'generating',
     'a',
     'summary',
     'from',
     'a',
     'single',
     'source',
     'document',
     ',',
     'while',
     'others',
     'can',
     'use',
     'multiple',
     'source',
     'documents',
     '(',
     'for',
     'example',
     ',',
     'a',
     'cluster',
     'of',
     'articles',
     'on',
     'the',
     'same',
     'topic',
     ')',
     '.',
     'This',
     'problem',
     'is',
     'called',
     'multi',
     '-',
     'document',
     'summarization',
     '.',
     'A',
     'related',
     'application',
     'is',
     'summarizing',
     'news',
     'articles',
     '.',
     'Imagine',
     'a',
     'system',
     ',',
     'which',
     'automatically',
     'pulls',
     'together',
     'news',
     'articles',
     'on',
     'a',
     'given',
     'topic',
     '(',
     'from',
     'the',
     'web',
     ')',
     ',',
     'and',
     'concisely',
     'represents',
     'the',
     'latest',
     'news',
     'as',
     'a',
     'summary',
     '.',
     '\n',
     'Image',
     'collection',
     'summarization',
     'is',
     'another',
     'application',
     'example',
     'of',
     'automatic',
     'summarization',
     '.',
     'It',
     'consists',
     'in',
     'selecting',
     'a',
     'representative',
     'set',
     'of',
     'images',
     'from',
     'a',
     'larger',
     'set',
     'of',
     'images.[4',
     ']',
     'A',
     'summary',
     'in',
     'this',
     'context',
     'is',
     'useful',
     'to',
     'show',
     'the',
     'most',
     'representative',
     'images',
     'of',
     'results',
     'in',
     'an',
     'image',
     'collection',
     'exploration',
     'system',
     '.',
     'Video',
     'summarization',
     'is',
     'a',
     'related',
     'domain',
     ',',
     'where',
     'the',
     'system',
     'automatically',
     'creates',
     'a',
     'trailer',
     'of',
     'a',
     'long',
     'video',
     '.',
     'This',
     'also',
     'has',
     'applications',
     'in',
     'consumer',
     'or',
     'personal',
     'videos',
     ',',
     'where',
     'one',
     'might',
     'want',
     'to',
     'skip',
     'the',
     'boring',
     'or',
     'repetitive',
     'actions',
     '.',
     'Similarly',
     ',',
     'in',
     'surveillance',
     'videos',
     ',',
     'one',
     'would',
     'want',
     'to',
     'extract',
     'important',
     'and',
     'suspicious',
     'activity',
     ',',
     'while',
     'ignoring',
     'all',
     'the',
     'boring',
     'and',
     'redundant',
     'frames',
     'captured']




```python
len(tokens)
```




    322




```python
punctuation # also called as noisy characters
```




    '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'




```python
doc
```




    There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on. The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.). The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query. Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.
    An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document. Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic). This problem is called multi-document summarization. A related application is summarizing news articles. Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.
    Image collection summarization is another application example of automatic summarization. It consists in selecting a representative set of images from a larger set of images.[4] A summary in this context is useful to show the most representative images of results in an image collection exploration system. Video summarization is a related domain, where the system automatically creates a trailer of a long video. This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions. Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured 




```python
#we have to calcualte the freaquency of each and every word, how many time word is repetation in text 

word_frequencies = {}

for word in doc:
    if word.text.lower() not in stopwords:
        if word.text.lower() not in punctuation:
            if word.text not in word_frequencies.keys():
                word_frequencies[word.text] = 1
            else:
                word_frequencies[word.text] += 1
```


```python
word_frequencies
#print(word_frequencies)
```




    {'broadly': 1,
     'types': 1,
     'extractive': 1,
     'summarization': 11,
     'tasks': 1,
     'depending': 2,
     'program': 1,
     'focuses': 2,
     'generic': 3,
     'obtaining': 1,
     'summary': 4,
     'abstract': 2,
     'collection': 3,
     'documents': 2,
     'sets': 1,
     'images': 3,
     'videos': 3,
     'news': 4,
     'stories': 1,
     'etc': 1,
     'second': 1,
     'query': 4,
     'relevant': 2,
     'called': 2,
     'based': 1,
     'summarizes': 1,
     'objects': 1,
     'specific': 1,
     'Summarization': 1,
     'systems': 1,
     'able': 1,
     'create': 1,
     'text': 1,
     'summaries': 2,
     'machine': 1,
     'generated': 1,
     'user': 1,
     'needs': 1,
     '\n': 2,
     'example': 3,
     'problem': 2,
     'document': 4,
     'attempts': 1,
     'automatically': 3,
     'produce': 1,
     'given': 2,
     'interested': 1,
     'generating': 1,
     'single': 1,
     'source': 2,
     'use': 1,
     'multiple': 1,
     'cluster': 1,
     'articles': 3,
     'topic': 2,
     'multi': 1,
     'related': 2,
     'application': 2,
     'summarizing': 1,
     'Imagine': 1,
     'system': 3,
     'pulls': 1,
     'web': 1,
     'concisely': 1,
     'represents': 1,
     'latest': 1,
     'Image': 1,
     'automatic': 1,
     'consists': 1,
     'selecting': 1,
     'representative': 2,
     'set': 2,
     'larger': 1,
     'images.[4': 1,
     'context': 1,
     'useful': 1,
     'results': 1,
     'image': 1,
     'exploration': 1,
     'Video': 1,
     'domain': 1,
     'creates': 1,
     'trailer': 1,
     'long': 1,
     'video': 1,
     'applications': 1,
     'consumer': 1,
     'personal': 1,
     'want': 2,
     'skip': 1,
     'boring': 2,
     'repetitive': 1,
     'actions': 1,
     'Similarly': 1,
     'surveillance': 1,
     'extract': 1,
     'important': 1,
     'suspicious': 1,
     'activity': 1,
     'ignoring': 1,
     'redundant': 1,
     'frames': 1,
     'captured': 1}




```python
len(word_frequencies) 
```




    103




```python
word_frequencies
```




    {'broadly': 1,
     'types': 1,
     'extractive': 1,
     'summarization': 11,
     'tasks': 1,
     'depending': 2,
     'program': 1,
     'focuses': 2,
     'generic': 3,
     'obtaining': 1,
     'summary': 4,
     'abstract': 2,
     'collection': 3,
     'documents': 2,
     'sets': 1,
     'images': 3,
     'videos': 3,
     'news': 4,
     'stories': 1,
     'etc': 1,
     'second': 1,
     'query': 4,
     'relevant': 2,
     'called': 2,
     'based': 1,
     'summarizes': 1,
     'objects': 1,
     'specific': 1,
     'Summarization': 1,
     'systems': 1,
     'able': 1,
     'create': 1,
     'text': 1,
     'summaries': 2,
     'machine': 1,
     'generated': 1,
     'user': 1,
     'needs': 1,
     '\n': 2,
     'example': 3,
     'problem': 2,
     'document': 4,
     'attempts': 1,
     'automatically': 3,
     'produce': 1,
     'given': 2,
     'interested': 1,
     'generating': 1,
     'single': 1,
     'source': 2,
     'use': 1,
     'multiple': 1,
     'cluster': 1,
     'articles': 3,
     'topic': 2,
     'multi': 1,
     'related': 2,
     'application': 2,
     'summarizing': 1,
     'Imagine': 1,
     'system': 3,
     'pulls': 1,
     'web': 1,
     'concisely': 1,
     'represents': 1,
     'latest': 1,
     'Image': 1,
     'automatic': 1,
     'consists': 1,
     'selecting': 1,
     'representative': 2,
     'set': 2,
     'larger': 1,
     'images.[4': 1,
     'context': 1,
     'useful': 1,
     'results': 1,
     'image': 1,
     'exploration': 1,
     'Video': 1,
     'domain': 1,
     'creates': 1,
     'trailer': 1,
     'long': 1,
     'video': 1,
     'applications': 1,
     'consumer': 1,
     'personal': 1,
     'want': 2,
     'skip': 1,
     'boring': 2,
     'repetitive': 1,
     'actions': 1,
     'Similarly': 1,
     'surveillance': 1,
     'extract': 1,
     'important': 1,
     'suspicious': 1,
     'activity': 1,
     'ignoring': 1,
     'redundant': 1,
     'frames': 1,
     'captured': 1}




```python
max_frequency = max(word_frequencies.values())
max_frequency 
```




    11




```python
#to get normalized/weighted frequencies you should devide all frequencies with 11
for word in word_frequencies.keys():
    word_frequencies[word] = word_frequencies[word]/max_frequency
```


```python
#print(word_frequencies)
word_frequencies
#this is the normalized frequencies of each word
```




    {'broadly': 0.09090909090909091,
     'types': 0.09090909090909091,
     'extractive': 0.09090909090909091,
     'summarization': 1.0,
     'tasks': 0.09090909090909091,
     'depending': 0.18181818181818182,
     'program': 0.09090909090909091,
     'focuses': 0.18181818181818182,
     'generic': 0.2727272727272727,
     'obtaining': 0.09090909090909091,
     'summary': 0.36363636363636365,
     'abstract': 0.18181818181818182,
     'collection': 0.2727272727272727,
     'documents': 0.18181818181818182,
     'sets': 0.09090909090909091,
     'images': 0.2727272727272727,
     'videos': 0.2727272727272727,
     'news': 0.36363636363636365,
     'stories': 0.09090909090909091,
     'etc': 0.09090909090909091,
     'second': 0.09090909090909091,
     'query': 0.36363636363636365,
     'relevant': 0.18181818181818182,
     'called': 0.18181818181818182,
     'based': 0.09090909090909091,
     'summarizes': 0.09090909090909091,
     'objects': 0.09090909090909091,
     'specific': 0.09090909090909091,
     'Summarization': 0.09090909090909091,
     'systems': 0.09090909090909091,
     'able': 0.09090909090909091,
     'create': 0.09090909090909091,
     'text': 0.09090909090909091,
     'summaries': 0.18181818181818182,
     'machine': 0.09090909090909091,
     'generated': 0.09090909090909091,
     'user': 0.09090909090909091,
     'needs': 0.09090909090909091,
     '\n': 0.18181818181818182,
     'example': 0.2727272727272727,
     'problem': 0.18181818181818182,
     'document': 0.36363636363636365,
     'attempts': 0.09090909090909091,
     'automatically': 0.2727272727272727,
     'produce': 0.09090909090909091,
     'given': 0.18181818181818182,
     'interested': 0.09090909090909091,
     'generating': 0.09090909090909091,
     'single': 0.09090909090909091,
     'source': 0.18181818181818182,
     'use': 0.09090909090909091,
     'multiple': 0.09090909090909091,
     'cluster': 0.09090909090909091,
     'articles': 0.2727272727272727,
     'topic': 0.18181818181818182,
     'multi': 0.09090909090909091,
     'related': 0.18181818181818182,
     'application': 0.18181818181818182,
     'summarizing': 0.09090909090909091,
     'Imagine': 0.09090909090909091,
     'system': 0.2727272727272727,
     'pulls': 0.09090909090909091,
     'web': 0.09090909090909091,
     'concisely': 0.09090909090909091,
     'represents': 0.09090909090909091,
     'latest': 0.09090909090909091,
     'Image': 0.09090909090909091,
     'automatic': 0.09090909090909091,
     'consists': 0.09090909090909091,
     'selecting': 0.09090909090909091,
     'representative': 0.18181818181818182,
     'set': 0.18181818181818182,
     'larger': 0.09090909090909091,
     'images.[4': 0.09090909090909091,
     'context': 0.09090909090909091,
     'useful': 0.09090909090909091,
     'results': 0.09090909090909091,
     'image': 0.09090909090909091,
     'exploration': 0.09090909090909091,
     'Video': 0.09090909090909091,
     'domain': 0.09090909090909091,
     'creates': 0.09090909090909091,
     'trailer': 0.09090909090909091,
     'long': 0.09090909090909091,
     'video': 0.09090909090909091,
     'applications': 0.09090909090909091,
     'consumer': 0.09090909090909091,
     'personal': 0.09090909090909091,
     'want': 0.18181818181818182,
     'skip': 0.09090909090909091,
     'boring': 0.18181818181818182,
     'repetitive': 0.09090909090909091,
     'actions': 0.09090909090909091,
     'Similarly': 0.09090909090909091,
     'surveillance': 0.09090909090909091,
     'extract': 0.09090909090909091,
     'important': 0.09090909090909091,
     'suspicious': 0.09090909090909091,
     'activity': 0.09090909090909091,
     'ignoring': 0.09090909090909091,
     'redundant': 0.09090909090909091,
     'frames': 0.09090909090909091,
     'captured': 0.09090909090909091}




```python
sentence_tokens = [sent for sent in doc.sents]
sentence_tokens
```




    [There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on.,
     The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.).,
     The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query.,
     Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.,
     An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document.,
     Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic).,
     This problem is called multi-document summarization.,
     A related application is summarizing news articles.,
     Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.,
     Image collection summarization is another application example of automatic summarization.,
     It consists in selecting a representative set of images from a larger set of images.[4],
     A summary in this context is useful to show the most representative images of results in an image collection exploration system.,
     Video summarization is a related domain, where the system automatically creates a trailer of a long video.,
     This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions.,
     Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured]




```python
len(sentence_tokens)
```




    15




```python
sentence_tokens
```




    [There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on.,
     The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.).,
     The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query.,
     Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.,
     An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document.,
     Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic).,
     This problem is called multi-document summarization.,
     A related application is summarizing news articles.,
     Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.,
     Image collection summarization is another application example of automatic summarization.,
     It consists in selecting a representative set of images from a larger set of images.[4],
     A summary in this context is useful to show the most representative images of results in an image collection exploration system.,
     Video summarization is a related domain, where the system automatically creates a trailer of a long video.,
     This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions.,
     Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured]




```python
# we are going to calculate the sentence score, to calculate the sentence score 
sentence_scores = {}

for sent in sentence_tokens:
    for word in sent:
        if word.text.lower() in word_frequencies.keys():
            if sent not in sentence_scores.keys():
                sentence_scores[sent] = word_frequencies[word.text.lower()]
            else:
                sentence_scores[sent] += word_frequencies[word.text.lower()]
        
```


```python
sentence_scores
```




    {There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on.: 2.818181818181818,
     The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.).: 3.9999999999999987,
     The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query.: 3.909090909090909,
     Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.: 3.2727272727272716,
     An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document.: 3.9999999999999996,
     Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic).: 2.545454545454545,
     This problem is called multi-document summarization.: 1.8181818181818183,
     A related application is summarizing news articles.: 1.0909090909090908,
     Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.: 2.9090909090909087,
     Image collection summarization is another application example of automatic summarization.: 2.909090909090909,
     It consists in selecting a representative set of images from a larger set of images.[4]: 1.1818181818181817,
     A summary in this context is useful to show the most representative images of results in an image collection exploration system.: 1.818181818181818,
     Video summarization is a related domain, where the system automatically creates a trailer of a long video.: 2.2727272727272725,
     This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions.: 1.1818181818181817,
     Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured: 1.4545454545454544}




```python
#lets say our case study was 30% sentence with maximum scores
from heapq import nlargest 
```


```python
select_length = int(len(sentence_tokens)*0.3)
select_length
```




    4




```python
#we have to select maximum 4 sentences out of all sentences 
summary = nlargest(select_length,sentence_scores, key = sentence_scores.get)
```


```python
summary
```




    [An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document.,
     The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.).,
     The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query.,
     Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.]




```python
sentence_scores
```




    {There are broadly two types of extractive summarization tasks depending on what the summarization program focuses on.: 2.818181818181818,
     The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.).: 3.9999999999999987,
     The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query.: 3.909090909090909,
     Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.: 3.2727272727272716,
     An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document.: 3.9999999999999996,
     Sometimes one might be interested in generating a summary from a single source document, while others can use multiple source documents (for example, a cluster of articles on the same topic).: 2.545454545454545,
     This problem is called multi-document summarization.: 1.8181818181818183,
     A related application is summarizing news articles.: 1.0909090909090908,
     Imagine a system, which automatically pulls together news articles on a given topic (from the web), and concisely represents the latest news as a summary.: 2.9090909090909087,
     Image collection summarization is another application example of automatic summarization.: 2.909090909090909,
     It consists in selecting a representative set of images from a larger set of images.[4]: 1.1818181818181817,
     A summary in this context is useful to show the most representative images of results in an image collection exploration system.: 1.818181818181818,
     Video summarization is a related domain, where the system automatically creates a trailer of a long video.: 2.2727272727272725,
     This also has applications in consumer or personal videos, where one might want to skip the boring or repetitive actions.: 1.1818181818181817,
     Similarly, in surveillance videos, one would want to extract important and suspicious activity, while ignoring all the boring and redundant frames captured: 1.4545454545454544}




```python
# if i need to combine these top 3 sentencs then 

final_summary = [word.text for word in summary]
```


```python
final_summary
```




    ['An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document.',
     'The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.).',
     'The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query.',
     'Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.\n']




```python
print(summary) # we get the final summary by our model
```

    [An example of a summarization problem is document summarization, which attempts to automatically produce an abstract from a given document., The first is generic summarization, which focuses on obtaining a generic summary or abstract of the collection (whether documents, or sets of images, or videos, news stories etc.)., The second is query relevant summarization, sometimes called query-based summarization, which summarizes objects specific to a query., Summarization systems are able to create both query relevant text summaries and generic machine-generated summaries depending on what the user needs.
    ]
    


```python

```
