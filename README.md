import streamlit as st
import openai
import os
from openai import OpenAI
os.environ["OPENAI_API_KEY"] = st.secrets['API_KEY']
client = OpenAI(
    api_key=os.environ.get("OPENAI_API_KEY"),

# Streamlit UI 구성
st.title("🗺️ 여행지 추천 관광 코스 생성기")
st.write("원하는 여행지를 입력하면 하루 추천 관광 코스를 만들어 드립니다!")

# 사용자 입력
destination = st.text_input("여행지를 입력하세요 (예: 서울, 파리, 뉴욕)")

# AI 추천 기능
if st.button("코스 추천받기"):
    if destination:
        prompt = f"나는 여행 플래너입니다. '{destination}'에서 하루 동안 방문할 수 있는 관광 코스를 만들어주세요. 시간대별로 명소를 추천하고, 각 명소에 대한 간단한 설명을 포함하세요."
        response = openai.Completion.create(
            engine="text-davinci-003",
            prompt=prompt,
            max_tokens=300
        )
        recommendations = response.choices[0].text.strip()
        st.subheader("추천된 관광 코스:")
        st.write(recommendations)
    else:
        st.warning("여행지를 입력해주세요!")
