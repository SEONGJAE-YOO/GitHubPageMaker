---
layout: page
current: page
cover:  assets/built/images/applied-text-analysis-with-python.png
navigation: True
title: applied text analysis with python - chapter 1 - Language and Computation
tags: [applied text analysis with python]  
class: post-template
subclass: 'post tag-applied text analysis with python'
author: SeongJae Yu  
sitemap:
lastmod: 2021-10-28
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---

# applied text analysis with python

# chapter 1 - Language and Computation

# Language Features

Consider a simple model that uses linguistic features to identify the predominant
gender in a piece of text. In 2013 Neal Caren, an assistant professor of Sociology at
the University of North Carolina Chapel Hill, wrote a blog post that investigated the
7
role of gender in news to determine if men and women come up in different contexts.
He applied a gender-based analysis of text to New York Times articles and determined
that in fact male and female words appeared in starkly different contexts, potentially
reinforcing gender biases.
What was particularly interesting about this analysis was the use of gendered words
to create a frequency-based score of maleness or femaleness. In order to implement a
similar analysis in Python, we can begin by building sets of words that differentiate
sentences about men and about women. For simplicity, we’ll say that a sentence can
have one of four states—it can be about men, about women, about both men and
women, or unknown (since sentences can be about neither men nor women, and also
because our MALE_WORDSand FEMALE_WORDSsets are not exhaustive):


언어적 특징을 사용하여 우세한 것을 식별하는 간단한 모델을 고려하십시오.
텍스트의 성별. 2013년 닐 카렌(Neal Caren)
University of North Carolina Chapel Hill은 조사한 블로그 게시물을 작성했습니다.
7
남성과 여성이 다른 맥락에서 등장하는지 여부를 결정하기 위해 뉴스에서 성별의 역할.
그는 텍스트에 대한 성별 기반 분석을 New York Times 기사에 적용하고 다음을 결정했습니다.
사실 남성과 여성의 단어는 완전히 다른 맥락에서 나타났고, 잠재적으로
성 편견 강화.
이 분석에서 특히 흥미로운 점은 성별 단어의 사용이었습니다.
남성 또는 여성의 빈도 기반 점수를 생성합니다. 구현하기 위해서는
Python에서 유사한 분석을 통해 차별화되는 단어 세트를 구축할 수 있습니다.
남성과 여성에 관한 문장. 단순화를 위해 우리는 문장이
네 가지 상태 중 하나가 있습니다. 남성, 여성, 남성 및
여성 또는 알 수 없음(문장은 남성이나 여성에 대한 것일 수 없으며 또한
MALE_WORDS 및 FEMALE_WORDS세트가 완전하지 않기 때문):


```python
import nltk
from collections import Counter

MALE = 'male'
FEMALE = 'female'
UNKNOWN = 'unknown'
BOTH = 'both'

MALE_WORDS = set([
    'guy','spokesman','chairman',"men's",'men','him',"he's",'his',
    'boy','boyfriend','boyfriends','boys','brother','brothers','dad',
    'dads','dude','father','fathers','fiance','gentleman','gentlemen',
    'god','grandfather','grandpa','grandson','groom','he','himself',
    'husband','husbands','king','male','man','mr','nephew','nephews',
    'priest','prince','son','sons','uncle','uncles','waiter','widower',
    'widowers'
])

FEMALE_WORDS = set([
    'heroine','spokeswoman','chairwoman',"women's",'actress','women',
    "she's",'her','aunt','aunts','bride','daughter','daughters','female',
    'fiancee','girl','girlfriend','girlfriends','girls','goddess',
    'granddaughter','grandma','grandmother','herself','ladies','lady',
    'mom','moms','mother','mothers','mrs','ms','niece','nieces',
    'priestess','princess','queens','she','sister','sisters','waitress',
    'widow','widows','wife','wives','woman'
])
```

Now that we have gender word sets, we need a method for assigning gender to a sen‐tence;
we'll create a genderize function that examines the numbers of words from a
sentence that appear in our MALE_WORDS list and in our FEMALE_WORDS list.


이제 성별 단어 세트가 있으므로, 성별을 문장에 할당하는 방법이 필요합니다.
MALE_WORDS 목록 및 FEMALE_WORDS 목록에서 나타난 문장으로 부터 단어의 수를 검사하는 성별화 함수를 만들 것입니다.


If a sentence has only MALE_WORDS, we’ll call it a male sentence, and if it has only
FEMALE_WORDS, we’ll call it a female sentence. If a sentence has nonzero counts for
both male and female words, we’ll call it both; and if it has zero male and zero female
words, we’ll call it unknown:


만약 문장에 MALE_WORDS개만 있으면 남성 문장이라고 하고
FEMALE_WORDS, 우리는 그것을 여성 문장이라고 부를 것입니다. 문장에 0이 아닌 개수가 있는 경우
남성과 여성 단어 모두, 우리는 그것을 둘 다 부를 것입니다. 남성이 0이고 여성이 0인 경우
우리는 그것을 알 수 없다고 부를 것입니다:


```python
def genderize(words):

    mwlen = len(MALE_WORDS.intersection(words))
    fwlen = len(FEMALE_WORDS.intersection(words))

    if mwlen > 0 and fwlen == 0:
        return MALE
    elif mwlen == 0 and fwlen > 0:
        return FEMALE
    elif mwlen > 0 and fwlen > 0:
        return BOTH
    else:
        return UNKNOWN
```

- Python Set intersection()

A = {2, 3, 5, 4}

B = {2, 5, 100}

C = {2, 3, 8, 9, 10}

print(B.intersection(A))

print(B.intersection(C))

print(A.intersection(C))

print(C.intersection(A, B))

___________________________________________________________
Output

{2, 5}
{2}
{2, 3}
{2}

We need a method for counting the frequency of gendered words and sentences
within the complete text of an article, which we can do with the collections.Countersclass, a built-in Python class.
The count_genderfunction takes a list of senten‐ces and applies the genderize function to evaluate the total number of gendered
words and gendered sentences.
Each sentence’s gender is counted and all words in the sentence are also considered as belonging to that gender:

컬렉션으로 할 수 있는 기사의 전체 텍스트 내에서
젠더화된 단어와 문장의 빈도를 계산하는 방법이 필요합니다.
내장 파이썬 클래스인 Countersclass. count_genderfunction은 보낸 목록을 취합니다.
총 젠더화 수를 평가하기 위해 젠더화 기능을 적용하고 적용합니다.
단어와 성별 문장. 각 문장의 성별이 계산되고 모든 단어가
문장도 해당 성별에 속하는 것으로 간주됩니다.


```python
def count_gender(sentences): #성별 수 계산하는 함수

    sents = Counter()
    words = Counter()

    for sentence in sentences:
        gender = genderize(sentence)
        sents[gender] += 1
        words[gender] += len(sentence)

    return sents, words
```

Finally, in order to engage our gender counters, we require some mechanism for
parsing the raw text of the articles into component sentences and words, and for this
we will use the NLTK library (which we’ll discuss further later in this chapter and in
the next) to break our paragraphs into sentences. With the sentences isolated, we can
then tokenize them to identify individual words and punctuation and pass the toke‐
nized text to our gender counters to print a document’s percent male, female, both, or
unknown


마지막으로 젠더 카운터를 사용하려면 몇 가지 메커니즘이 필요합니다.
기사의 원시 텍스트를 구성 문장과 단어로 구문 분석하고 이를 위해
우리는 NLTK 라이브러리를 사용할 것입니다(이 장의 뒷부분과
다음) 단락을 문장으로 나눕니다. 문장을 분리하면
그런 다음 개별 단어와 구두점을 식별하고 토큰을 전달하도록 토큰화합니다.
문서의 남성, 여성, 둘 다 또는
알려지지 않은


```python
def parse_gender(text):

    sentences = [
        [word.lower() for word in nltk.word_tokenize(sentence)]
        for sentence in nltk.sent_tokenize(text)
    ]

    sents, words = count_gender(sentences)
    total = sum(words.values())

    for gender, count in words.items():
        pcent = (count / total) * 100
        nsents = sents[gender]

        print(
            "{:0.3f}% {} ({} sentences)".format(pcent, gender, nsents)
        )
```

Running our parse_gender function on an article from the New York Times entitled “Rehearse, Ice Feet, Repeat: The Life of a New York City Ballet Corps Dancer” yields
the following, unsurprising results:


New York Times의 기사에서 parse_gender 함수 실행
"리허설, 얼음 발, 반복: 뉴욕시 발레단 댄서의 삶" 산출량
다음과 같은 놀라운 결과:

# ballet.txt 파일 내용

With apologies to James Brown, the hardest working people in show business may well be ballet dancers. And at New York City Ballet, none work harder than the dancers in its lowest rank, the corps de ballet. During the first week of the company’s winter season, Claire Kretzschmar, 24, a rising corps member, danced in all seven performances, appearing in five ballets, sometimes changing costumes at intermission to dance two roles in a night.

But her work onstage did not even begin to capture the stamina required to be in the corps. Spending a week shadowing Ms. Kretzschmar was exhausting — she gave new meaning to the idea of being on your feet all day. Twelve-hour days at the David H. Koch Theater, the company’s Lincoln Center home, were hardly unusual: Company class each morning was followed by back-to-back-to-back rehearsals, with occasional breaks for costume fittings or physical therapy, and then by the hair-makeup-costume-dance routine of daily performances.

Video
Every day, Claire Kretzschmar of the New York City Ballet goes from class to rehearsal to performance at a breathless pace. Dance alongside her busy day. CREDIT PHOTOGRAPH BY SASHA ARUTYUNOVA FOR THE NEW YORK TIMES. TECHNOLOGY BY SAMSUNG. PUBLISH DATE JANUARY 27, 2017
This weekend will be even more frenetic. Ms. Kretzschmar will appear in seven ballets from Friday evening to Sunday afternoon, when she faces a new test: taking on the title role of the Sleepwalker in George Balanchine’s eerie, proto-goth ballet “La Sonnambula.” Balanchine, one of ballet’s most important choreographers, was a founder of City Ballet, and remains its guiding spirit more than three decades after his death.

Being in City Ballet’s corps is not like being a member of a chorus line, or a backup singer. The company promotes almost all of its stars, the principal dancers, from within. So while the corps is expected to be able to move in startling unison, like a school of fish, and to assemble in straight lines and keep all rippling swan arms parallel, its 54 members are also competing for bigger roles and promotions.

“There is an element of competition, and people get different opportunities, but everybody just wants to do their best onstage, and everyone wants each other to just do their best onstage,” Ms. Kretzschmar said during a break between rehearsals last week. “We have all experienced so many extreme highs and lows that it’s almost that you have to bond with this group of people.”

Here are scenes from one week in the busy life of a corps member.

Image
Ms. Kretzschmar at a morning company dance class, followed by back-to-back-to-back rehearsals. CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
Tuesday, Opening Day
10:09 A.M. COMPANY CLASS, ‘NOTHING FANCY’ Ms. Kretzschmar arrives early in the large studio on the Koch’s fifth floor, stakes out her regular spot at the barre in the back corner, and wraps her toes in paper towels before slipping on her point shoes. Dancers soon fill the room. Unity Phelan and Brittany Pollack bring their dogs.

Peter Martins, the company’s ballet master in chief, strides in, coffee cup in hand, and puts the whole company through its paces, as the rehearsal pianist plays Gershwin. Later he says: “George Balanchine used to tell me, ‘You know what class is about, dear?’ He said, ‘What we do is we brush teeth, we clean teeth every morning. Nothing fancy: We just keep the essentials pure.’”

11:30 A.M. REHEARSAL STUDIO After class, Ms. Kretzschmar — who studied ballet while growing up in North Carolina, came to New York to study at the School of American Ballet, the company’s official school, and joined the corps in 2011 — stays in the studio to observe a rehearsal of a dance she is learning, and then watches a new Pontus Lidberg ballet for which she is an understudy. At 1 p.m., she finally gets on her toes for a rehearsal of “Fearful Symmetries,” a demanding, propulsive ballet choreographed by Mr. Martins that she was given a principal role in. She pauses to ask how to time a climactic gesture in which she rests her head on her partner’s palm. “It’s a Hail Mary,” the ballet master, Kathleen Tracey, tells her. “Hope for the best!”

Image
Ms. Kretzschmar in a rehearsal at the David H. Koch Theater. “There is an element of competition,” she said, “but everybody just wants to do their best onstage, and everyone wants each other to just do their best onstage.” CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
2:20 P.M. SLEEPWALKING ONSTAGE The wig needs combing. Few characters in ballet make as lasting an impression as the Sleepwalker, an enigmatic character with long, flowing hair who makes her unseeing entrance on point, holding a candle. But when Ms. Kretzschmar first steps out onstage in her new wig, the action briefly stops, as the ballet masters confer with Suzy Alvarez, the company’s hair and makeup supervisor, about how they want the wig to look.

It was the wig that first tipped Ms. Kretzschmar off that she was getting the part; even before the casting was announced she was summoned to a fitting. “I don’t think they would give me a wig if I wasn’t going to do it,” she said.

The dancers work on one-year contracts. Corps members earn roughly $1,100 to $2,100 a week, depending on seniority, and are typically paid for 37 to 39 weeks a year. Since ballet careers are short, many are in a hurry to make their marks.

Giving corps members a shot at big roles like this is part of the ethos of City Ballet. “You test them,” Mr. Martins said. “What are they made of? Am I wrong, am I right? You throw little bones to them and see how they react.”

Image
Ms. Kretzschmar at a dress rehearsal of “Swan Lake.” CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
3:10 P.M. COSTUME FITTING: TUTUS AND HOODIES City Ballet’s costume shop — acres of tulle and tutus — is tucked away on the eighth floor of the Rose Building at Lincoln Center. Fittings here offer a brief respite from the theater and a breath of fresh air. Ms. Kretzschmar tries on the black hoodie, skirt and high-top sneakers she will wear in Justin Peck’s new ballet, “The Times Are Racing.” Then it was back to the theater for one more rehearsal and then opening night, where she danced in Balanchine’s “Firebird,” the last ballet on the evening’s triple bill.

Image
A 15-minute physical therapy session. Her left foot, which she injured in the past, “gets tighter quicker than the right one,” Ms. Kretzschmar says. CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
Wednesday
1:30 P.M. PHYSICAL THERAPY Ballet is a physically punishing, sometimes dangerous business: Injuries are all too common and can lead to missed performances and careers cut short. Ms. Kretzschmar signs up for 15-minute physical therapy sessions in a small space resembling a doctor’s office on the fifth floor of the theater. She lies on a table and Taryn Khong, the therapist, manipulates her left foot, which Ms. Kretzschmar has injured in the past. “It gets tighter quicker than the right one,” she said.

4:18 P.M.: ‘FEARFUL SYMMETRIES’ REHEARSAL Ms. Kretzschmar begins her third stage rehearsal of the day. Mr. Martins, who choreographed the piece, makes a request: At one point, when she thrusts her leg behind her, he wants her to bend it to one side, forming a sort of arabesque-with-a-twist. Ms. Tracey, the ballet master leading the rehearsal, demonstrates. “That, exactly,” Mr. Martins says. Ms. Kretzschmar tries once more to execute the twisting step, which some dancers call “crack your back” because it looks as if it might.

Image
A makeup session before dancing “Swan Lake.” CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
6:40 P.M. HAIR AND MAKEUP: HEAD, MEET BUN Time to become a swan. “Swan Lake” is one of the most famous ballets of them all, and Balanchine’s unusual one-act take on it has plenty of dancing for the corps members playing swans. Sitting at a light-bulb-ringed mirror in the hair and makeup room, Ms. Kretzschmar applies eyeliner and puts her hair in a neat bun. A hairdresser calls across the room to ask all the swans to make sure that some hair can be seen beneath their matching black headdresses, which come to a point in the center of their foreheads — lest they start to look like a member of “The Munsters” family with widow’s peaks.

“Good evening everyone, this is your half-hour call,” a voice says over a loudspeaker.

7 P.M. DRESSING ROOM: LIVING OUT OF A THEATER CASE In the fourth-floor dressing room she shares with other corps women, Ms. Kretzschmar pins on her headdress. This is her corner of the theater. It consists of a mirror, a hanging closet shelf crammed with leotards, tights and clothes, and her theater case — one of the old-fashioned black rectangular cases that dancers live out of and tour with. Hers sits open on a luggage rack; as is traditional, the inside lid is decorated with personal mementos.

Image
Claire Kretzschmar of New York City Ballet preparing for a rehearsal of “The Four Temperaments” at the David H. Koch Theater. CREDITPHOTOGRAPH BY SASHA ARUTYUNOVA FOR THE NEW YORK TIMES. TECHNOLOGY BY SAMSUNG.
7:25 P.M. THE WINGS The space off stage right resembles a mini ballet studio: There is a barre, a small dance floor, and an area for dancers to fix their shoes. Although she is not in the evening’s first ballet, Ms. Kretzschmar arrives before it begins and uses a rasplike tool to scuff the soles of her point shoes so they won’t slip.

“I don’t like to feel rushed before the show, because that just makes me more stressed,” she explains later. “For me, I take that time so I can kind of calm down.”

Next to her, Tiler Peck, one of the company’s biggest stars, prepares her own shoes. The company’s music director, Andrew Litton, arrives in his tuxedo before heading down to the pit. “Hey Tiler, merde,” he says, employing the earthy French expression ballet dancers use to wish each other luck. “Any last requests?”

Then, although she has been dancing much of the day, she warms up again, seemingly oblivious to the performance taking place onstage 30 feet away.

Image
Ms. Kretzschmar in “Swan Lake.” CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
8:13 P.M. SHOWTIME Now it’s time to use those muscles: first as a swan in “Swan Lake,” the evening’s second ballet, and, after changing into a modern leotard, in Balanchine’s “The Four Temperaments.”

9:40 P.M. CURTAIN DOWN, HOME LEAVE After the curtain falls, she goes home for a dinner of leftover pulled pork and macaroni and cheese, and a little television — which she watches with her feet in an ice bucket.

Friday
12:02 P.M. PECK REHEARSALS: HOODIES ON As Donald J. Trump is being inaugurated in Washington and marchers are preparing for a weekend of mass protests, Ms. Kretzschmar is onstage rehearsing the new Justin Peck ballet — which feels timely, with its dancers clad in shirts and hoodies emblazoned with messages including “Protest,” “Act” and “Unite.” Experimenting, she flips up her hood as she dances. “Try it!” Mr. Peck says encouragingly. “I don’t mind if it comes off.”

Saturday
6:30 P.M. SLEEPWALKING BETWEEN SHOWS Although she has just danced in a matinee and has another performance that evening, Ms. Kretzschmar requests another “Sonnambula” rehearsal sandwiched between shows. Glenn Keenan, the ballet master, walks her through it again and again.

Ms. Keenan asks the pianist, Elaine Chelton, to pick up at the moment the Sleepwalker touches the body of the Poet. “Do you have where she bumps into the body?” she asks. Ms. Chelton scans her piano score, which is annotated with all manner of steps and stage actions. “I have where she steps over,” she replies, and begins playing a little before that.

Image
Ms. Kretzschmar, right, during a rehearsal for Justin Peck’s “Scherzo Fantastique” at the David H. Koch Theater. CREDITSASHA ARUTYUNOVA FOR THE NEW YORK TIMES
Sunday
5:15 P.M. ONE WEEK DOWN, TWO DEBUTS TO COME After dancing two more roles at the matinee — as a male-eating insect in Jerome Robbins’s “The Cage,” and in the corps of Mr. Peck’s “Scherzo Fantastique” — her week comes to an end. She pauses to reflect on the week ahead: On Monday, the company’s day off, she will start a class at Fordham, where she is close to earning a degree in communications, with a minor in English. Then back to dancing, and the debuts of her two big roles, which she said she would keep honing between all her other roles.

“I want to feel really prepared,” she said, adding that she would take care not to overdo it. “I try to take a day-by-day, hour-by-hour approach.”


```python
if __name__ == '__main__':
    with open('C:/Users/MyCom//jupyter-tutorial/applied text analysis with python/data/ballet.txt', 'r',encoding='UTF8') as f:
        parse_gender(f.read())
```

    39.269% unknown (48 sentences)
    52.994% female (38 sentences)
    4.393% both (2 sentences)
    3.344% male (3 sentences)


The scoring function here takes into account the length of the sentence in terms of
the number of words it contains. Therefore even though there are fewer total female
sentences, over 50% of the article is female. Extensions of this technique can analyze
words that are in female sentences versus in male sentences to see if there are any
auxiliary terms that are by default associated with male and female genders. We can
see that this analysis is relatively easy to implement in Python, and Caren found his
results very striking:



여기에서 채점 기능은 다음과 같이 문장의 길이를 고려합니다.
포함하는 단어의 수입니다. 따라서 전체 여성의 수가 적음에도 불구하고
문장의 50% 이상이 여성입니다. 이 기술의 확장은 다음을 분석할 수 있습니다.
여성 문장 대 남성 문장의 단어가 있는지 확인하기 위해
기본적으로 남성 및 여성 성별과 관련된 보조 용어입니다. 우리는 할 수 있습니다
이 분석은 Python에서 구현하기가 비교적 쉽고 Caren은 자신의
매우 놀라운 결과:


