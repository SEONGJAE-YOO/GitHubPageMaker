---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Natural Language Processing (NLP) in Python with 8 Projects - Text summarization
tags: [udemy]  
class: post-template
subclass: 'post tag-udemy'  
author: SeongJae Yu  
sitemap:
lastmod: 2021-11-10
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include udemy.html %}

## Text summarization


```python
text = """ Maria Sharapova has basically no friends as tennis players on the WTA Tour. The Russian player has no problems in openly speaking about it and in a recent interview she said: ‘I don’t really hide any feelings too much.
I think everyone knows this is my job here. When I’m on the courts or when I’m on the court playing, I’m a competitor and I want to beat every single person whether they’re in the locker room or across the net.
So I’m not the one to strike up a conversation about the weather and know that in the next few minutes I have to go and try to win a tennis match.
I’m a pretty competitive girl. I say my hellos, but I’m not sending any players flowers as well. Uhm, I’m not really friendly or close to many players.
I have not a lot of friends away from the courts.’ When she said she is not really close to a lot of players, is that something strategic that she is doing? Is it different on the men’s tour than the women’s tour? ‘No, not at all.
I think just because you’re in the same sport doesn’t mean that you have to be friends with everyone just because you’re categorized, you’re a tennis player, so you’re going to get along with tennis players.
I think every person has different interests. I have friends that have completely different jobs and interests, and I’ve met them in very different parts of my life.
I think everyone just thinks because we’re tennis players we should be the greatest of friends. But ultimately tennis is just a very small part of what we do.
There are so many other things that we’re interested in, that we do. """
```


```python
len(text)
```




    1563



# 1) Importing the libraries and Dataset

# string.punctuation in Python
# 참고 사이트- [https://www.geeksforgeeks.org/string-punctuation-in-python/](https://www.geeksforgeeks.org/string-punctuation-in-python/)


```python
import spacy
from spacy.lang.en.stop_words import STOP_WORDS
from string import punctuation
```


```python
nlp = spacy.load("en_core_web_sm")
```


```python
doc = nlp(text)
```


```python
tokens = [token.text for token in doc]
print(tokens)
```

    [' ', 'Maria', 'Sharapova', 'has', 'basically', 'no', 'friends', 'as', 'tennis', 'players', 'on', 'the', 'WTA', 'Tour', '.', 'The', 'Russian', 'player', 'has', 'no', 'problems', 'in', 'openly', 'speaking', 'about', 'it', 'and', 'in', 'a', 'recent', 'interview', 'she', 'said', ':', '‘', 'I', 'do', 'n’t', 'really', 'hide', 'any', 'feelings', 'too', 'much', '.', '\n', 'I', 'think', 'everyone', 'knows', 'this', 'is', 'my', 'job', 'here', '.', 'When', 'I', '’m', 'on', 'the', 'courts', 'or', 'when', 'I', '’m', 'on', 'the', 'court', 'playing', ',', 'I', '’m', 'a', 'competitor', 'and', 'I', 'want', 'to', 'beat', 'every', 'single', 'person', 'whether', 'they', '’re', 'in', 'the', 'locker', 'room', 'or', 'across', 'the', 'net', '.', '\n', 'So', 'I', '’m', 'not', 'the', 'one', 'to', 'strike', 'up', 'a', 'conversation', 'about', 'the', 'weather', 'and', 'know', 'that', 'in', 'the', 'next', 'few', 'minutes', 'I', 'have', 'to', 'go', 'and', 'try', 'to', 'win', 'a', 'tennis', 'match', '.', '\n', 'I', '’m', 'a', 'pretty', 'competitive', 'girl', '.', 'I', 'say', 'my', 'hellos', ',', 'but', 'I', '’m', 'not', 'sending', 'any', 'players', 'flowers', 'as', 'well', '.', 'Uhm', ',', 'I', '’m', 'not', 'really', 'friendly', 'or', 'close', 'to', 'many', 'players', '.', '\n', 'I', 'have', 'not', 'a', 'lot', 'of', 'friends', 'away', 'from', 'the', 'courts', '.', '’', 'When', 'she', 'said', 'she', 'is', 'not', 'really', 'close', 'to', 'a', 'lot', 'of', 'players', ',', 'is', 'that', 'something', 'strategic', 'that', 'she', 'is', 'doing', '?', 'Is', 'it', 'different', 'on', 'the', 'men', '’s', 'tour', 'than', 'the', 'women', '’s', 'tour', '?', '‘', 'No', ',', 'not', 'at', 'all', '.', '\n', 'I', 'think', 'just', 'because', 'you', '’re', 'in', 'the', 'same', 'sport', 'does', 'n’t', 'mean', 'that', 'you', 'have', 'to', 'be', 'friends', 'with', 'everyone', 'just', 'because', 'you', '’re', 'categorized', ',', 'you', '’re', 'a', 'tennis', 'player', ',', 'so', 'you', '’re', 'going', 'to', 'get', 'along', 'with', 'tennis', 'players', '.', '\n', 'I', 'think', 'every', 'person', 'has', 'different', 'interests', '.', 'I', 'have', 'friends', 'that', 'have', 'completely', 'different', 'jobs', 'and', 'interests', ',', 'and', 'I', '’ve', 'met', 'them', 'in', 'very', 'different', 'parts', 'of', 'my', 'life', '.', '\n', 'I', 'think', 'everyone', 'just', 'thinks', 'because', 'we', '’re', 'tennis', 'players', 'we', 'should', 'be', 'the', 'greatest', 'of', 'friends', '.', 'But', 'ultimately', 'tennis', 'is', 'just', 'a', 'very', 'small', 'part', 'of', 'what', 'we', 'do', '.', '\n', 'There', 'are', 'so', 'many', 'other', 'things', 'that', 'we', '’re', 'interested', 'in', ',', 'that', 'we', 'do', '.']



```python
punctuation =  punctuation + '\n'
```


```python
punctuation
```




    '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~\n'




```python

```

# 2) Text Cleaning

# Create Word Frequency Counter


```python
word_freq = {}

stop_words = list(STOP_WORDS)

for word in doc:
  if word.text.lower() not in  stop_words:
    if word.text.lower() not in punctuation:
      if word.text not in word_freq.keys(): # 단어 몇번 나왔는지 횟수 
        word_freq[word.text] = 1
      else:
        word_freq[word.text] += 1  

```


```python
print(word_freq)
```

    {' ': 1, 'Maria': 1, 'Sharapova': 1, 'basically': 1, 'friends': 5, 'tennis': 6, 'players': 6, 'WTA': 1, 'Tour': 1, 'Russian': 1, 'player': 2, 'problems': 1, 'openly': 1, 'speaking': 1, 'recent': 1, 'interview': 1, 'said': 2, '‘': 2, 'hide': 1, 'feelings': 1, 'think': 4, 'knows': 1, 'job': 1, 'courts': 2, 'court': 1, 'playing': 1, 'competitor': 1, 'want': 1, 'beat': 1, 'single': 1, 'person': 2, 'locker': 1, 'room': 1, 'net': 1, 'strike': 1, 'conversation': 1, 'weather': 1, 'know': 1, 'minutes': 1, 'try': 1, 'win': 1, 'match': 1, 'pretty': 1, 'competitive': 1, 'girl': 1, 'hellos': 1, 'sending': 1, 'flowers': 1, 'Uhm': 1, 'friendly': 1, 'close': 2, 'lot': 2, 'away': 1, '’': 1, 'strategic': 1, 'different': 4, 'men': 1, 'tour': 2, 'women': 1, 'sport': 1, 'mean': 1, 'categorized': 1, 'going': 1, 'interests': 2, 'completely': 1, 'jobs': 1, 'met': 1, 'parts': 1, 'life': 1, 'thinks': 1, 'greatest': 1, 'ultimately': 1, 'small': 1, 'things': 1, 'interested': 1}



```python
max_freq = max(word_freq.values())  # 6을 변수에 저장
```


```python
for word in word_freq.keys():
  word_freq[word] = word_freq[word] / max_freq
```


```python
print(word_freq)
```

    {' ': 0.16666666666666666, 'Maria': 0.16666666666666666, 'Sharapova': 0.16666666666666666, 'basically': 0.16666666666666666, 'friends': 0.8333333333333334, 'tennis': 1.0, 'players': 1.0, 'WTA': 0.16666666666666666, 'Tour': 0.16666666666666666, 'Russian': 0.16666666666666666, 'player': 0.3333333333333333, 'problems': 0.16666666666666666, 'openly': 0.16666666666666666, 'speaking': 0.16666666666666666, 'recent': 0.16666666666666666, 'interview': 0.16666666666666666, 'said': 0.3333333333333333, '‘': 0.3333333333333333, 'hide': 0.16666666666666666, 'feelings': 0.16666666666666666, 'think': 0.6666666666666666, 'knows': 0.16666666666666666, 'job': 0.16666666666666666, 'courts': 0.3333333333333333, 'court': 0.16666666666666666, 'playing': 0.16666666666666666, 'competitor': 0.16666666666666666, 'want': 0.16666666666666666, 'beat': 0.16666666666666666, 'single': 0.16666666666666666, 'person': 0.3333333333333333, 'locker': 0.16666666666666666, 'room': 0.16666666666666666, 'net': 0.16666666666666666, 'strike': 0.16666666666666666, 'conversation': 0.16666666666666666, 'weather': 0.16666666666666666, 'know': 0.16666666666666666, 'minutes': 0.16666666666666666, 'try': 0.16666666666666666, 'win': 0.16666666666666666, 'match': 0.16666666666666666, 'pretty': 0.16666666666666666, 'competitive': 0.16666666666666666, 'girl': 0.16666666666666666, 'hellos': 0.16666666666666666, 'sending': 0.16666666666666666, 'flowers': 0.16666666666666666, 'Uhm': 0.16666666666666666, 'friendly': 0.16666666666666666, 'close': 0.3333333333333333, 'lot': 0.3333333333333333, 'away': 0.16666666666666666, '’': 0.16666666666666666, 'strategic': 0.16666666666666666, 'different': 0.6666666666666666, 'men': 0.16666666666666666, 'tour': 0.3333333333333333, 'women': 0.16666666666666666, 'sport': 0.16666666666666666, 'mean': 0.16666666666666666, 'categorized': 0.16666666666666666, 'going': 0.16666666666666666, 'interests': 0.3333333333333333, 'completely': 0.16666666666666666, 'jobs': 0.16666666666666666, 'met': 0.16666666666666666, 'parts': 0.16666666666666666, 'life': 0.16666666666666666, 'thinks': 0.16666666666666666, 'greatest': 0.16666666666666666, 'ultimately': 0.16666666666666666, 'small': 0.16666666666666666, 'things': 0.16666666666666666, 'interested': 0.16666666666666666}


# 3) Sentence tokenization


```python
sent_tokens = [sent for sent in doc.sents]
print(sent_tokens)
```

    [ Maria Sharapova has basically no friends as tennis players on the WTA Tour., The Russian player has no problems in openly speaking about it and in a recent interview she said: ‘I don’t really hide any feelings too much., 
    I think everyone knows this is my job here., When I’m on the courts or when I’m on the court playing, I’m a competitor and I want to beat every single person whether they’re in the locker room or across the net., 
    , So I’m not the one to strike up a conversation about the weather and know that in the next few minutes I have to go and try to win a tennis match., 
    I’m a pretty competitive girl., I say my hellos, but I’m not sending any players flowers as well., Uhm, I’m not really friendly or close to many players., 
    , I have not a lot of friends away from the courts.’, When she said she is not really close to a lot of players, is that something strategic that she is doing?, Is it different on the men’s tour than the women’s tour?, ‘No, not at all., 
    , I think just because you’re in the same sport doesn’t mean that you have to be friends with everyone just because you’re categorized, you’re a tennis player, so you’re going to get along with tennis players., 
    , I think every person has different interests., I have friends that have completely different jobs and interests, and I’ve met them in very different parts of my life., 
    I think everyone just thinks because we’re tennis players we should be the greatest of friends., But ultimately tennis is just a very small part of what we do., 
    , There are so many other things that we’re interested in, that we do.]


# Calculate Sentence Score


```python
sent_score = {}
```


```python
for sent in sent_tokens:
    for word in sent:
        print(word)
```


    Maria
    Sharapova
    has
    basically
    no
    friends
    as
    tennis
    players
    on
    the
    WTA
    Tour
    .
    The
    Russian
    player
    has
    no
    problems
    in
    openly
    speaking
    about
    it
    and
    in
    a
    recent
    interview
    she
    said
    :
    ‘
    I
    do
    n’t
    really
    hide
    any
    feelings
    too
    much
    .
    
    
    I
    think
    everyone
    knows
    this
    is
    my
    job
    here
    .
    When
    I
    ’m
    on
    the
    courts
    or
    when
    I
    ’m
    on
    the
    court
    playing
    ,
    I
    ’m
    a
    competitor
    and
    I
    want
    to
    beat
    every
    single
    person
    whether
    they
    ’re
    in
    the
    locker
    room
    or
    across
    the
    net
    .
    
    
    So
    I
    ’m
    not
    the
    one
    to
    strike
    up
    a
    conversation
    about
    the
    weather
    and
    know
    that
    in
    the
    next
    few
    minutes
    I
    have
    to
    go
    and
    try
    to
    win
    a
    tennis
    match
    .
    
    
    I
    ’m
    a
    pretty
    competitive
    girl
    .
    I
    say
    my
    hellos
    ,
    but
    I
    ’m
    not
    sending
    any
    players
    flowers
    as
    well
    .
    Uhm
    ,
    I
    ’m
    not
    really
    friendly
    or
    close
    to
    many
    players
    .
    
    
    I
    have
    not
    a
    lot
    of
    friends
    away
    from
    the
    courts
    .
    ’
    When
    she
    said
    she
    is
    not
    really
    close
    to
    a
    lot
    of
    players
    ,
    is
    that
    something
    strategic
    that
    she
    is
    doing
    ?
    Is
    it
    different
    on
    the
    men
    ’s
    tour
    than
    the
    women
    ’s
    tour
    ?
    ‘
    No
    ,
    not
    at
    all
    .
    
    
    I
    think
    just
    because
    you
    ’re
    in
    the
    same
    sport
    does
    n’t
    mean
    that
    you
    have
    to
    be
    friends
    with
    everyone
    just
    because
    you
    ’re
    categorized
    ,
    you
    ’re
    a
    tennis
    player
    ,
    so
    you
    ’re
    going
    to
    get
    along
    with
    tennis
    players
    .
    
    
    I
    think
    every
    person
    has
    different
    interests
    .
    I
    have
    friends
    that
    have
    completely
    different
    jobs
    and
    interests
    ,
    and
    I
    ’ve
    met
    them
    in
    very
    different
    parts
    of
    my
    life
    .
    
    
    I
    think
    everyone
    just
    thinks
    because
    we
    ’re
    tennis
    players
    we
    should
    be
    the
    greatest
    of
    friends
    .
    But
    ultimately
    tennis
    is
    just
    a
    very
    small
    part
    of
    what
    we
    do
    .
    
    
    There
    are
    so
    many
    other
    things
    that
    we
    ’re
    interested
    in
    ,
    that
    we
    do
    .



```python
for sent in sent_tokens:
  for word in sent:
    if word.text.lower()in word_freq.keys():
      if sent not in sent_score.keys():
        sent_score[sent] = word_freq[word.text.lower()]
      else:
        sent_score[sent] += word_freq[word.text.lower()]  

```


```python
print(sent_score)
```

    { Maria Sharapova has basically no friends as tennis players on the WTA Tour.: 3.5000000000000004, The Russian player has no problems in openly speaking about it and in a recent interview she said: ‘I don’t really hide any feelings too much.: 2.1666666666666665, 
    I think everyone knows this is my job here.: 0.9999999999999999, When I’m on the courts or when I’m on the court playing, I’m a competitor and I want to beat every single person whether they’re in the locker room or across the net.: 2.1666666666666665, So I’m not the one to strike up a conversation about the weather and know that in the next few minutes I have to go and try to win a tennis match.: 2.333333333333333, 
    I’m a pretty competitive girl.: 0.5, I say my hellos, but I’m not sending any players flowers as well.: 1.5, Uhm, I’m not really friendly or close to many players.: 1.5, I have not a lot of friends away from the courts.’: 1.8333333333333335, When she said she is not really close to a lot of players, is that something strategic that she is doing?: 2.1666666666666665, Is it different on the men’s tour than the women’s tour?: 1.6666666666666665, ‘No, not at all.: 0.3333333333333333, I think just because you’re in the same sport doesn’t mean that you have to be friends with everyone just because you’re categorized, you’re a tennis player, so you’re going to get along with tennis players.: 5.5, I think every person has different interests.: 1.9999999999999998, I have friends that have completely different jobs and interests, and I’ve met them in very different parts of my life.: 3.3333333333333326, 
    I think everyone just thinks because we’re tennis players we should be the greatest of friends.: 3.833333333333333, But ultimately tennis is just a very small part of what we do.: 1.3333333333333335, There are so many other things that we’re interested in, that we do.: 0.3333333333333333}



```python

```

# 4) Select 30% sentences with maximum score


```python
from heapq import nlargest
```


```python
len(sent_score) * 0.3
```




    5.3999999999999995




```python
6 #반올림하여 6으로 설정하자 (nlargest , n 값을 6으로 설정)
```




    6



# 5) Getting the Summary


```python
summary = nlargest(n = 6, iterable= sent_score, key = sent_score.get)
```


```python
print(summary)
```

    [I think just because you’re in the same sport doesn’t mean that you have to be friends with everyone just because you’re categorized, you’re a tennis player, so you’re going to get along with tennis players., 
    I think everyone just thinks because we’re tennis players we should be the greatest of friends.,  Maria Sharapova has basically no friends as tennis players on the WTA Tour., I have friends that have completely different jobs and interests, and I’ve met them in very different parts of my life., So I’m not the one to strike up a conversation about the weather and know that in the next few minutes I have to go and try to win a tennis match., The Russian player has no problems in openly speaking about it and in a recent interview she said: ‘I don’t really hide any feelings too much.]



```python
final_summary = [word.text for word in summary]
```


```python
print(final_summary)
```

    ['I think just because you’re in the same sport doesn’t mean that you have to be friends with everyone just because you’re categorized, you’re a tennis player, so you’re going to get along with tennis players.', '\nI think everyone just thinks because we’re tennis players we should be the greatest of friends.', ' Maria Sharapova has basically no friends as tennis players on the WTA Tour.', 'I have friends that have completely different jobs and interests, and I’ve met them in very different parts of my life.', 'So I’m not the one to strike up a conversation about the weather and know that in the next few minutes I have to go and try to win a tennis match.', 'The Russian player has no problems in openly speaking about it and in a recent interview she said: ‘I don’t really hide any feelings too much.']



```python
summary = " ".join(final_summary)
```


```python
print(summary)
```

    I think just because you’re in the same sport doesn’t mean that you have to be friends with everyone just because you’re categorized, you’re a tennis player, so you’re going to get along with tennis players. 
    I think everyone just thinks because we’re tennis players we should be the greatest of friends.  Maria Sharapova has basically no friends as tennis players on the WTA Tour. I have friends that have completely different jobs and interests, and I’ve met them in very different parts of my life. So I’m not the one to strike up a conversation about the weather and know that in the next few minutes I have to go and try to win a tennis match. The Russian player has no problems in openly speaking about it and in a recent interview she said: ‘I don’t really hide any feelings too much.



```python
len(summary)
```




    791




```python
len(summary)/ len(text)
```




    0.5060780550223928


