import numpy
import xml.etree.ElementTree
from xml.dom import minidom

dictionary = {",":'COMMA',".":'PERIOD',"?":'QMARK',"\"":'QUOTES',"-":'DASH',"!":'EXCLAMATION'}

f = open('data/PF--_E_00026_SE_01_T_01.fln')
x = f.read()	

count = 0
punct = 0
inp = []
for i in range(1,len(x)):
	if x[i-1]=='<' and x[i]=='w':
		count += 1
		inp.append('w')
	elif x[i-1]=='<' and x[i]=='p':
		punct += 1
		inp.append('p')

f = minidom.parse('data/PF--_E_00026_SE_01_T_01.fln')
items = f.getElementsByTagName('w')

words = []
for s in items:
	tup = (str(s.childNodes[0].nodeValue.encode('utf-8')),str(s.attributes['pos'].value.encode('utf-8')),str(s.attributes['lemma'].value.encode('utf-8')))
#	if s.childNodes[0].nodeValue[0]>='A' and s.childNodes[0].nodeValue[0]<='Z':
#		print s.childNodes[0].nodeValue
	words.append(tup)
	
	
items = f.getElementsByTagName('p')
punctuations = []
for s in items:
	tup = dictionary[s.childNodes[0].nodeValue]
	punctuations.append(tup)
	
print len(words)
print len(punctuations)

final = []
wi = 0
pi = 0
epsilon = 0

for i in range(len(inp)-1):
	if i==len(inp)-1 and inp[i]=='w':
		temp = words[wi] + ('Epsilon',)
		final.append(temp)
		wi += 1
		epsilon += 1
	elif inp[i]=='w' and inp[i+1]=='w':
		temp = words[wi] + ('Epsilon',)
		final.append(temp)
		wi += 1
		epsilon += 1
	elif i<len(inp)-2 and inp[i]=='w' and inp[i+1]=='p' and inp[i+2]=='p':
		temp = words[wi] + (punctuations[pi],)
		final.append(temp)
		pi += 2
		wi += 1
		i+=2
	elif inp[i]=='w' and inp[i+1]=='p':
		temp = words[wi] + (punctuations[pi],)
		final.append(temp)
		pi += 1
		wi += 1
		i+=1
		
print 'Input size:',len(final)
print 'Number of empty punctuations:',epsilon

fo = open('train_file.txt','w')
for i in range(len(final)):
	fo.write(final[i][0])
	fo.write('\t')
	fo.write(final[i][1])
	fo.write('\t')
	fo.write(final[i][2])
	fo.write('\t')
	fo.write(final[i][3])
	fo.write('\n')

fo.close()



