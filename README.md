# newsweb-
NewsTop20 is a sleek and user-friendly news application designed to provide users with the top 20 headlines from their selected country. The app aims to keep users informed with the most current and relevant news from around the world, tailored to their location and preferences.

#code starts from here 
import streamlit as st
import requests

# Constants
API_KEY = '1601a3deef304678bc83f51009951aba'
BASE_URL = 'https://newsapi.org/v2/top-headlines'

# Function to fetch news data
def fetch_news(country='us', topic=''):
    try:
        url = f"{BASE_URL}?country={country}&q={topic}&apiKey={API_KEY}"
        st.write(f"Requesting URL: {url}")  # Debugging: Print the request URL
        response = requests.get(url)
        response.raise_for_status()  # Raise an HTTPError for bad responses
        data = response.json()
        st.write("Response data: ", data)  # Debugging: Print the response data
        if data.get('status') == 'ok':
            return data.get('articles', [])
        else:
            st.error(f"Error from NewsAPI: {data.get('message')}")
            return []
    except requests.exceptions.RequestException as e:
        st.error(f"An error occurred while fetching news: {e}")
        return []

# Streamlit app
def main():
    st.title("News App")
    st.write("Top 20 news headlines by country or topic")

    country = st.selectbox("Select Country", ['us', 'uk', 'in', 'ca', 'au', 'de'])
    topic = st.text_input("Enter Topic")

    if st.button("Get News"):
        articles = fetch_news(country, topic)
        if articles:
            for article in articles[:20]:
                st.subheader(article['title'])
                if article.get('urlToImage'):
                    st.image(article['urlToImage'])
                st.write(article['description'])
                st.write(f"[Read more]({article['url']})")
        else:
            st.write("No articles found")

if __name__ == "__main__":
    main()
