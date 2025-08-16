# Book Recommender
This project was built to recommend books for reader based on what's in their mind, by exploring a large dataset, use vector search, and some text classification.

## Dataset:
The dataset used in this project contains many books with their related data such as isbn13, author, title, description, etc.....
| isbn13       | isbn10     | title          | subtitle  | authors                           | categories                     | thumbnail                                                                 | description                                                                 | published_year | average_rating | num_pages | ratings_count |
|--------------|------------|----------------|-----------|-----------------------------------|--------------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|----------------|----------------|-----------|---------------|
| 9780002005883 | 0002005883 | Gilead         | NaN       | Marilynne Robinson                | Fiction                        | http://books.google.com/books/content?id=KQZCP...                         | A NOVEL THAT READERS and critics have been eag...                           | 2004.0         | 3.85           | 247.0     | 361.0         |
| 9780002261982 | 0002261987 | Spider's Web   | A Novel   | Charles Osborne;Agatha Christie   | Detective and mystery stories  | http://books.google.com/books/content?id=gA5GP...                         | A new 'Christie for Christmas' -- a full-lengt...                           | 2000.0         | 3.83           | 241.0     | 5164.0        |
| 9780006163831 | 0006163831 | The One Tree   | NaN       | Stephen R. Donaldson              | American fiction               | http://books.google.com/books/content?id=OmQaw...                         | Volume Two of Stephen Donaldson's acclaimed se...                           | 1982.0         | 3.97           | 479.0     | 172.0         |
| 9780006178736 | 0006178731 | Rage of angels | NaN       | Sidney Sheldon                    | Fiction                        | http://books.google.com/books/content?id=FKo2T...                         | A memorable, mesmerizing heroine Jennifer -- b...                           | 1993.0         | 3.93           | 512.0     | 29532.0       |
| 9780006280897 | 0006280897 | The Four Loves | NaN       | Clive Staples Lewis               | Christian life                 | http://books.google.com/books/content?id=XhQ5X...                         | Lewis' work on the nature of love divides love...                           | 2002.0         | 4.15           | 170.0     | 33684.0       |

Link to the dataset: https://www.kaggle.com/datasets/dylanjcastillo/7k-books-with-metadata

## Pipeline:

### 1. Data Exploration:
During this stage, the data was cleaned and processed to be more helpful in building the recommender by handling misiing values and create tagged descriptions.

