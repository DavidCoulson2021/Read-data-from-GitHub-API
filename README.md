# Read-data-from-GitHub-API
A course on Linkedin Learning: "Python for Data Science Tips, Tricks, & Techniques" has out of date content (Python 2).  From the Q&A section it is apparent Learners are frustrated as they, to include myself, lack the skill to fix the plane while it's flying.  This repository consolidates the work of several of people posted into the courses Q&A section (to include my minor fixes to the print() statements).   This should work with Jupyter using Python 3.

The specific section of the course is "Read data from GitHub API":
https://www.linkedin.com/learning/python-for-data-science-tips-tricks-techniques/read-data-from-github-api?contextUrn=urn%3Ali%3AlyndaLearningPath%3A5b61ea25498e580437e51859&resume=false&u=0

The code is found below.

import requests
response = requests.get('https://api.github.com/repos/bsullins/data/contents/monthlySalesbyCategoryMultiple.json')

import json
resp_json = json.loads(response.text)

##Here is the 1st big fix##
#val = json.loads(resp_json['content'].decode('base64'))
import base64
val = json.loads(base64.b64decode(resp_json['content']))

from pprint import pprint as pp
pp(val)

# print the keys and values at the third level
for a in val['contents']:
    for b in a['monthlySales']:
        for key, value in b.items():
            print(key + ": ", value)

response = requests.get('https://api.github.com/repos/bsullins/data/contents/MonthlySales.csv')

response_json = json.loads(response.text)

##Here is the 2nd big fix##
#csv_val = base64.b64decode(response_json['content'])
csv_val = base64.b64decode(response_json['content'].encode('ascii')).decode('ascii')


##If I recall correctly then I removed "unicode" from a portion of the following text##
# using csv.DictReader needs a filestream so we're making an String IO and passing a unicode string in
# then reading the stream and adding each dictionary to the dictionary list
import csv
import io
csv_dict = csv.DictReader(io.StringIO((csv_val)))
dict_list = []
for a in csv_dict:
    dict_list.append(a)

for a in dict_list:
    print(a)

#print keys and values
for a in dict_list:
    for key, value in a.items():
        print(key + ": ", value)
    print('\n')
