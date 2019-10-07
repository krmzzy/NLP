#!/usr/bin/env python
# coding: utf-8

# In[1]:


# 罗姆剃刀原理
# Pattern Based AI 


# In[33]:



hello_rules = '''
say_hello = names hello tail
nemes = name names | name
name = Jhon | Mike | 老梁 | 老刘
hello = 你好 | 您来啦 | 快请进
tail = 呀 | ！
'''


# In[43]:


rules = dict() # key is the @statement, value is  @experssion


# In[ ]:


'add = number + number'


# In[ ]:


stmt_split = '='
or_split = '|'

for line in hell_rules.split('\n'):
    if not line: continue
        #skip the empty line
        
        stmt,expr = line.split(stmt_split)
        
        print(stmt, expr.split(or_split))
        
        rules[stmt] = expr.split(or_split)


# In[80]:


rules


# In[83]:


hello_rules


# In[86]:


rules


# In[ ]:


def generate(grammar_rule, target):
    if target in grammar_rule:
        candidates = grammar_rule[target]
        candidate = random.choice(candidates)
        return ' '.join(generate(grammar_rule, target=c.strio()))


# In[4]:


def name():
    return random.choice('Jhon | Mike | 老梁'.split('|'))


# In[11]:


def hello():
    return random.choice('你好 | 您来啦 | 快请进'.split('|'))


# In[70]:


hello()


# In[87]:


def generate(grammar_rule, target):
    if target in grammar_rule: #name
        candidates = grammar_rule[target] # ['name names', 'name']
        candidate = random.choice(candidates) #'name names', 'naeme'
        return ' '.join(generate(grammar_rule, target=c.strip()) for c in candidate)
    else:
        return target


# In[89]:


generate(rules, target='say_hello')


# In[ ]:


def get_generation_by_gram(grammar_str: str, target, stmt_split='=' , or_split='\'):
    
    rules = dict() #key is the @statenment, value is @expression
    for line in grammar_str.split('\n'):
        if not line: continue
        # skip the empty line
      # print(line)
        stmt,expr = line.split(stmt_split)
        
        rules[stmt.strip()] = expr.split(or_split)
                           
    generated = grnerate(rules, target=target)
                           
    return generated


# In[90]:


simple_grammar = """
sentence => noun_phrase verb_phrase
noun_phrase => Article Adj* noun
Asj* => null | Adj Adj* noun
verb_phrase => verb noun_phrase
Article => 一个 | 这个
noun => 女人 | 篮球 | 桌子 | 小猫
verb => 看着 | 坐在 | 听着 | 看见
Adj => 蓝色的 | 好看的 | 小小的
"""


# In[4]:


get_generation_by_gram(simple_grammar, target='sentence', stmt_split='=>')


# In[96]:


simpel_programming = '''
if_stmt => if ( cond ) { stmt }
cond => var of var
op => | == | < | >= | <=
stmt => assign | if_stmt
assiqn => var = var
var => char var | char
char => a |  b | c |  d |
'''


# In[99]:


simpel_programming


# In[3]:


for i in range(20):
    print(get_generation_by_gram(simpel_programming, target='if_stmt', stmt_split='=>'))


# In[ ]:





# # Data Driven
# # 编译原理
# # 1990年 第一篇机器学习真正应用的论文， Data Driven
# # 期望我们的程序，能够根据我们输入的数据，自行进行处理。而不是说，数据一变，我们的程序也要随之进行改变。
# # 智能客服机器人小美
# # 微软小冰
# # 建行的机器人，中行的机器人
# # 文本的分析匹配
# 
# # 只要你发现有大量的if-else, 大量的规则要做的时候

# In[ ]:





# # language model
# # lnput:Sentence(w1..wn)
# # Output:Pribability(0-1)
# 2-Gram

# $$ pr(sentence) = pr(w_1 \cdot w_2 \cdots w_n) = \prod \orac{count(w_i, w_{i+1})}{count(v_i)}$$

# In[11]:


from nltk.corpus import PlaintextCorpusReader


# In[7]:


coupus= "//Users/krmzzy/jupyters_and_slides/2019-summer/article_9k.txt"


# In[8]:


FILE = open(coupus).read()


# In[9]:


len(FILE)


# In[12]:


def generate_by_pro(text_corpus,length=20):
    return ''.join(random.sample(text_corpus, length))


# In[15]:


FILE[:500]


# In[16]:


import jieba


# In[17]:


list(jieba.cut('一加手机5要做市面最轻薄'))


# In[25]:


max_length = 1000

sub_file = FILE[:max_length]


# In[26]:


def cut(string):
    return list(jieba.cut(string))


# In[45]:


TOKENS = cut(sub_file)


# In[46]:


TOKENS[:1231]


# In[ ]:





# In[28]:


len(TOKENS)


# In[29]:


from collections import Counter


# In[30]:


get_ipython().run_line_magic('matplotlib', 'inline')


# In[31]:


words_count = Counter(TOKENS)


# In[47]:


words_count.most_common(100)


# In[ ]:





# In[40]:


words_with_fre = [f for w, f in words_count.most_common()]


# In[41]:


import matplotlib.pyplot as plt


# In[44]:


words_count.most_common(10)


# In[42]:


words_with_fre[:10]


# In[ ]:


import munpy as np


# In[52]:


plt.plot(words_with_fre)


# # # 在大量的文本中，出现次数第二多的单词，它出现的概率是出现最高单词的1/2，出现频率第三高的最高的单词的1/3，1000=>1/1000

# In[53]:


_2_gram_words = [
    TOKENS[i] + TOKENS[i+1] for i in range(len(TOKENS)-1)
]


# In[54]:


_2_gram_words[:10]


# In[55]:


_2_gram_word_counts = Counter(_2_gram_words)


# In[65]:


words_count.most_common()[-1][-1]


# In[68]:


def get_1_gram_count(word):
    if word in words_count: return words_count[word]
    else:
        return words_count.most_common()[-1][-1]


# In[69]:


def get_2_gram_count(word):
    if word in _2_gram_word_counts: return _2_gram_word_counts[word]
    else:
        return _2_gram_word_counts.most_common()[-1][-1]


# In[80]:


def get_gram_count(word, wc):
    if word in wc: return wc[word]
    else:
        return wc.most_common()[-1][-1]


# In[81]:


get_gram_count('xxx', words_count)


# In[82]:


get_gram_count('xxx', _2_gram_word_counts)


# In[ ]:





# In[96]:


def two_gram_model(sentence):
    # 2-gram langau
    tokens = cut(sentence)
    
    probability = 1
    for i in range(len(tokens)-1):
        word = tokens[i]
        next_word = tokens[i+1]
        
        _two_gram_c = get_gram_count(word+next_word, _2_gram_word_counts)
        _one_gram_c = get_gram_count(next_word, words_count)
        pro = _two_gram_c / _one_gram_c
        
        probability *= pro
        
    return probability


# In[97]:


two_gram_model('名著麦田里的守望者中')


# In[98]:


two_gram_model('他毕业于清华大学')


# In[99]:


two_gram_model('毕业于他琴湖大学')


# In[100]:


two_gram_model('爷奶奶一样的老人赞许')


# # 有一些错误： AI： More Data, Better Result
# # 2-gram => 3-gram
