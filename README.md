# Book Recommender System
=====================================

This repository contains a book recommender system that uses two different techniques to suggest books:

 1. Popularity-Based Recommender System

2. Collaborative Filtering Recommender System

The project is built using three datasets:

- [Books Dataset](https://github.com/anonymousCAVEMAN/book-recommender-system/blob/master/data/Books.csv) – Contains metadata about books.

- [Users Dataset](https://github.com/anonymousCAVEMAN/book-recommender-system/blob/master/data/Users.csv) – Contains user who have rated

- [Ratings Dataset](https://github.com/anonymousCAVEMAN/book-recommender-system/blob/master/data/Ratings.csv) – Contains ratings given by users for various books.
# Features
-----------
- Top 50 Books: The popularity-based recommender suggests the top 50 books based on the highest number of ratings.

- Collaborative Filtering: This system uses user interaction to recommend books. It focuses on users who have made more than 200 ratings (top reviewers) from these shortlisted books again those Books with a minimum of 50 ratings are considered for recommendation.

# Approach
-----------
## Popularity-Based Recommender

-Group the ratings by Book-Title and count the number of ratings.

-Sort the books by the highest number of ratings to identify the top 50.

## Collaborative Filtering Recommender

- Filter users with more than 200 ratings:
```bash
rating_With_name.groupby('User-ID').count()['Book-Rating'] > 200

```
- Filter books with more than 50 ratings:
```bash
filtered_rating.groupby('Book-Title').count()['Book-Rating'] > 50
```

- Create a pivot table to represent user-book interactions:
```bash
pt = final_rating.pivot_table(index='Book-Title', columns='User-ID', values='Book-Rating')
```

- Calculate similarity scores using cosine similarity:
```bash
similarity_score = cosine_similarity(pt.fillna(0))
```

- Recommendation Function

This function recommends books based on similarity to a given book:
```bash
def recommend(book_name):
    # Fetch index of the given book
    index = np.where(pt.index == book_name)[0][0]
    
    # Sort books by similarity
    similar_items = sorted(list(enumerate(similarity_score[index])), key=lambda x: x[1], reverse=True)[1:5]
    
    data = []
    for i in similar_items:
        item = []
        temp_df = books[books['Book-Title'] == pt.index[i[0]]]
        item.extend(list(temp_df.drop_duplicates('Book-Title')['Book-Title'].values))
        item.extend(list(temp_df.drop_duplicates('Book-Title')['Book-Author'].values))
        item.extend(list(temp_df.drop_duplicates('Book-Title')['Image-URL-M'].values))
        data.append(item)
    
    return data
```
# Web Interface

A simple Flask web application is developed to demonstrate both recommender systems. Users can input a book title to get recommendations or browse the top 50 books based on popularity.

## Installation

- Install the required dependencies:
```bash
pip install -r requirements.txt
```
- Run the Flask app:
```bash
python app.py
```
# Usage
- Visit http://127.0.0.1:5000/ in your browser.
- look at navbar there will be recommend books option select it and make search
- Use the search bar to enter a book title and get recommendations.
- Browse the top 50 books listed on the homepage.
# Acknowledgements
 - [Flask Documentation](https://flask.palletsprojects.com/en/stable/)
 - [cosine-smilarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html)
 - [youtube](https://www.youtube.com/watch?v=1YoD0fg3_EM&t=2669s&ab_channel=CampusX)

# Future Enhancements

- Implement hybrid recommender systems.
- Improve performance by optimizing similarity calculations.
- Add more filtering options for personalized recommendations.


