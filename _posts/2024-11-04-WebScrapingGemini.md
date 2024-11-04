---
layout: post
title: "웹 스크래핑 + 구글 Gemini"
subtitle: "Web Scraping with Google Gemini"
author: "Seuthoot"
header-img: "img/gemini.jpg"
header-mask: 0.2
tags:
  - blog
  - AI
  - Web Scraping
  - Gemini
---

처음으로 개발관련 포스트를 올려본다.

사실 영어공부 겸 Medium에서 포스트를 읽다가, 웹 스크래핑과 구글 gemini를 활용하는 포스트를 발견해서

그대로 따라 해보려고 한다. (영어를 공부하는 이유가 영어로 된 정보를 얻기 위해서 이니, 미리 연습해보려 한다)

[미디움 포스트 링크](https://medium.com/nerd-for-tech/web-scraping-with-google-gemini-0b4a45765794)

------------------------------------------------------------------------------

# 구글 AI API KEY

우선 구글 api 키가 필요하다.

![](/img/in-post/WebScrapingGemini01.jpg)

[구글 AI 스튜디오](https://ai.google.dev/aistudio?source=post_page-----0b4a45765794--------------------------------&hl=ko)

방법은 생각보다 간단하다. 로그인 후, API 가져오기를 클릭해서 받으면 된다.

![](/img/in-post/WebScrapingGemini02.jpg)


`
import streamlit as st
import requests
from bs4 import BeautifulSoup
import os
import google.generativeai as genai
`


`
st.title("Proposal Calls") # Title for the page

os.environ['GOOGLE_API_KEY'] = "********************************"
genai.configure(api_key = os.environ['GOOGLE_API_KEY'])

model = genai.GenerativeModel('gemini-pro')
`


'
def read_input():
  # dictionary of all the links to be webscraped.
  # You can add more if you want to
   links = {
       "1":["DST","https://dst.gov.in/call-for-proposals"],
       "2":["BIRAC","https://birac.nic.in/cfp.php"]
   }
   for i in range(1,3):
       url = links[str(i)][1] # Get URL of each organization
       r = requests.get(url) # Request for data
       soup = BeautifulSoup(r.text, 'html.parser') # Parse the HTML elements
       data = soup.text # Get raw data in string format
       link = soup.find_all('a', href=True) # Get list of all links on the site in html formet
       l = ""
       for a in link:
           l = l +"\n"+ a['href'][1:] # Get the actual links
      # Create a query
       query = data + "name of organization is"+links[str(i)][0]+ "Jumbled links of calls for proposals:"+l+"\n Create a table with the following columns: Call for proposals or joint call for proposals along with respective link, opening date, closing date and the name of the organization."
       llm_function(query)
'


위 코드는 스크래핑을 하는 코드인데, 

`
links = {
    "1": ["네이버", "https://www.naver.com/"],
    "2": ["다음", "https://www.daum.net/"]
}
`
이해를 위해서 익숙한 네이버와 다음으로 변경하고 결과를 확인했다.
