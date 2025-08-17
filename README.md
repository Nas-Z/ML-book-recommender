# Book Recommender
A system that recommends books based on what readers have in mind. It leverages a large dataset of books, applies vector search for semantic similarity, and incorporates text classification techniques to deliver relevant and personalized recommendations.

## Dataset:
The project uses a large-scale book dataset that includes essential metadata such as:
- ISBN-13
- Author
- Title
- Description
- Additional attributes (e.g., categories, publication year, etc.)

This information forms the basis for building embeddings, performing vector search, and applying classification techniques for book recommendations.
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
In this stage, the raw dataset was cleaned and preprocessed to ensure high-quality input for the recommender system. The main steps included:
- Handling missing values (e.g., incomplete book metadata).
- Normalizing text fields such as titles and descriptions.
- Creating tagged descriptions with isbn13, which were later used for embedding generation and classification.

| isbn13       | isbn10     | title              | authors                           | categories                | thumbnail                                                                 | description                                                                 | published_year | average_rating | num_pages | ratings_count | title_and_subtitle     | tagged_description                                                                 |
|--------------|------------|--------------------|-----------------------------------|---------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|----------------|----------------|-----------|---------------|------------------------|----------------------------------------------------------------------------------|
| 9780002005883 | 0002005883 | Gilead             | Marilynne Robinson                | Fiction                   | http://books.google.com/books/content?id=KQZCP...                         | A NOVEL THAT READERS and critics have been eag...                           | 2004.0         | 3.85           | 247.0     | 361.0         | Gilead                 | 9780002005883 A NOVEL THAT READERS and critics...                               |
| 9780002261982 | 0002261987 | Spider's Web       | Charles Osborne;Agatha Christie   | Detective and mystery stories | http://books.google.com/books/content?id=gA5GP...                         | A new 'Christie for Christmas' -- a full-lengt...                           | 2000.0         | 3.83           | 241.0     | 5164.0        | Spider's Web: A Novel  | 9780002261982 A new 'Christie for Christmas' -...                              |
| 9780006178736 | 0006178731 | Rage of angels     | Sidney Sheldon                    | Fiction                   | http://books.google.com/books/content?id=FKo2T...                         | A memorable, mesmerizing heroine Jennifer -- b...                           | 1993.0         | 3.93           | 512.0     | 29532.0       | Rage of angels         | 9780006178736 A memorable, mesmerizing heroine...                               |
| 9780006280897 | 0006280897 | The Four Loves     | Clive Staples Lewis               | Christian life            | http://books.google.com/books/content?id=XhQ5X...                         | Lewis' work on the nature of love divides love...                           | 2002.0         | 4.15           | 170.0     | 33684.0       | The Four Loves         | 9780006280897 Lewis' work on the nature of lov...                               |
| 9780006280934 | 0006280935 | The Problem of Pain | Clive Staples Lewis              | Christian life            | http://books.google.com/books/content?id=Kk-uV...                         | "In The Problem of Pain, C.S. Lewis, one of th...                           | 2002.0         | 4.09           | 176.0     | 37569.0       | The Problem of Pain    | 9780006280934 "In The Problem of Pain, C.S. Le...                               |


### 2. Vector Search:
At this stage, a Chroma vector database was created using the processed book descriptions. This enabled the system to perform semantic retrieval and recommend books that are contextually related to a user’s query.

For example, given the query:

“A book to teach children about nature”

The vector search retrieves books with descriptions most semantically aligned to the query:

| id   | isbn13       | isbn10     | title                               | authors                          | categories        | thumbnail                                                                 | description                                                                 | published_year | average_rating | num_pages | ratings_count | title_and_subtitle                     | tagged_description                                                                 |
|------|--------------|------------|-------------------------------------|----------------------------------|------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|----------------|----------------|-----------|---------------|---------------------------------------|----------------------------------------------------------------------------------|
| 3214 | 9780689861130 | 0689861133 | Moo, Baa, la la La!                 | Sandra Boynton                   | Animal sounds    | http://books.google.com/books/content?id=Gz40A...                         | Children will love joining in and imitating th...                           | 2004.0         | 4.20           | 14.0      | 28261.0       | Moo, Baa, la la La!                   | 9780689861130 Children will love joining in an...                               |
| 3747 | 9780786808069 | 0786808063 | Baby Einstein: Neighborhood Animals | Marilyn Singer;Julie Aigner-Clark | Juvenile Fiction | http://books.google.com/books/content?id=X9a4P...                         | Children will discover the exciting world of t...                           | 2001.0         | 3.89           | 16.0      | 180.0         | Baby Einstein: Neighborhood Animals    | 9780786808069 Children will discover the excit...                               |
| 3748 | 9780786808373 | 0786808373 | Baby Einstein: Birds                | Julie Aigner-Clark               | Juvenile Fiction | http://books.google.com/books/content?id=0jxHP...                         | Introducing your baby to birds, cats, dogs, an...                           | 2002.0         | 3.78           | 20.0      | 9.0           | Baby Einstein: Birds                  | 9780786808373 Introducing your baby to birds, ...                               |
| 3749 | 9780786808380 | 0786808381 | Baby Einstein: Babies               | Julie Aigner-Clark               | Juvenile Fiction | http://books.google.com/books/content?id=jv4NA...                         | Introduce your babies to birds, cats, dogs, an...                           | 2002.0         | 4.03           | 20.0      | 29.0          | Baby Einstein: Babies                 | 9780786808380 Introduce your babies to birds, ...                               |
| 3750 | 9780786808397 | 078680839X | Baby Einstein: Dogs                 | Julie Aigner-Clark               | Juvenile Fiction | http://books.google.com/books/content?id=qut8t...                         | Introduce your baby to birds, cats, dogs, and ...                           | 2002.0         | 3.81           | 20.0      | 26.0          | Baby Einstein: Dogs                   | 9780786808397 Introduce your baby to birds, ca...                               |

### 3. Text Classification:
To simplify the diversity of book categories, zero-shot classification was applied to assign each book into one of the following predefined categories:
- Fiction
- Nonfiction
- Children’s Fiction
- Children’s Nonfiction

The model used for this task was facebook/bart-large-mnli from Hugging Face, which allows classification without requiring labeled training data. This ensured that each book was mapped into a consistent and manageable category set, improving the quality of recommendations.

| isbn13       | isbn10     | title              | authors                           | categories                | thumbnail                                                                 | description                                                                 | published_year | average_rating | num_pages | ratings_count | title_and_subtitle     | tagged_description                                                                 | simple_categories |
|--------------|------------|--------------------|-----------------------------------|---------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|----------------|----------------|-----------|---------------|------------------------|----------------------------------------------------------------------------------|------------------|
| 9780002005883 | 0002005883 | Gilead             | Marilynne Robinson                | Fiction                   | http://books.google.com/books/content?id=KQZCP...                         | A NOVEL THAT READERS and critics have been eag...                           | 2004.0         | 3.85           | 247.0     | 361.0         | Gilead                 | 9780002005883 A NOVEL THAT READERS and critics...                               | Fiction          |
| 9780002261982 | 0002261987 | Spider's Web       | Charles Osborne;Agatha Christie   | Detective and mystery stories | http://books.google.com/books/content?id=gA5GP...                         | A new 'Christie for Christmas' -- a full-lengt...                           | 2000.0         | 3.83           | 241.0     | 5164.0        | Spider's Web: A Novel  | 9780002261982 A new 'Christie for Christmas' -...                              | Fiction          |
| 9780006178736 | 0006178731 | Rage of angels     | Sidney Sheldon                    | Fiction                   | http://books.google.com/books/content?id=FKo2T...                         | A memorable, mesmerizing heroine Jennifer -- b...                           | 1993.0         | 3.93           | 512.0     | 29532.0       | Rage of angels         | 9780006178736 A memorable, mesmerizing heroine...                               | Fiction          |
| 9780006280897 | 0006280897 | The Four Loves     | Clive Staples Lewis               | Christian life            | http://books.google.com/books/content?id=XhQ5X...                         | Lewis' work on the nature of love divides love...                           | 2002.0         | 4.15           | 170.0     | 33684.0       | The Four Loves         | 9780006280897 Lewis' work on the nature of lov...                               | Nonfiction       |
| 9780006280934 | 0006280935 | The Problem of Pain | Clive Staples Lewis              | Christian life            | http://books.google.com/books/content?id=Kk-uV...                         | "In The Problem of Pain, C.S. Lewis, one of th...                           | 2002.0         | 4.09           | 176.0     | 37569.0       | The Problem of Pain    | 9780006280934 "In The Pr


### 4. Sentiment Analysis:
In this stage, each book’s description was analyzed to determine its emotional tone. The process involved:
- Splitting the description into individual sentences.
- Using a text classification model to predict the intensity of each emotion per sentence.
- Selecting the highest score for each one of the following categories:
  anger, disgust, fear, joy, sadness, surprise, neutral

The model used was j-hartmann/emotion-english-distilroberta-base from Hugging Face. This allowed the recommender to filter or suggest books not only by topic but also by emotional tone, enhancing personalization.

| isbn13       | isbn10     | title              | authors                        | categories                   | thumbnail                                                                 | description                                                                 | published_year | average_rating | num_pages | title_and_subtitle     | tagged_description                                                                 | simple_categories | anger    | disgust  | fear     | joy      | sadness  | surprise | neutral  |
|--------------|------------|------------------|--------------------------------|-------------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|----------------|----------------|-----------|------------------------|----------------------------------------------------------------------------------|------------------|---------|---------|---------|---------|---------|---------|---------|
| 9780002005883 | 0002005883 | Gilead            | Marilynne Robinson             | Fiction                      | http://books.google.com/books/content?id=KQZCP...                         | A NOVEL THAT READERS and critics have been eag...                           | 2004.0         | 3.85           | 247       | Gilead                 | 9780002005883 A NOVEL THAT READERS and critics...                               | Fiction          | 0.064134 | 0.273592 | 0.928168 | 0.932798 | 0.646216 | 0.967158 | 0.729602 |
| 9780002261982 | 0002261987 | Spider's Web      | Charles Osborne;Agatha Christie | Detective and mystery stories | http://books.google.com/books/content?id=gA5GP...                         | A new 'Christie for Christmas' -- a full-lengt...                           | 2000.0         | 3.83           | 241       | Spider's Web: A Novel  | 9780002261982 A new 'Christie for Christmas' -...                              | Fiction          | 0.612620 | 0.348285 | 0.942528 | 0.704422 | 0.887939 | 0.111690 | 0.252546 |
| 9780006178736 | 0006178731 | Rage of angels    | Sidney Sheldon                 | Fiction                      | http://books.google.com/books/content?id=FKo2T...                         | A memorable, mesmerizing heroine Jennifer -- b...                           | 1993.0         | 3.93           | 512       | Rage of angels         | 9780006178736 A memorable, mesmerizing heroine...                               | Fiction          | 0.064134 | 0.104007 | 0.972321 | 0.767238 | 0.549477 | 0.111690 | 0.078765 |
| 9780006280897 | 0006280897 | The Four Loves    | Clive Staples Lewis            | Christian life               | http://books.google.com/books/content?id=XhQ5X...                         | Lewis' work on the nature of love divides love...                           | 2002.0         | 4.15           | 170       | The Four Loves         | 9780006280897 Lewis' work on the nature of lov...                               | Nonfiction       | 0.351484 | 0.150722 | 0.360706 | 0.251881 | 0.732684 | 0.111690 | 0.078765 |
| 9780006280934 | 0006280935 | The Problem of Pain | Clive Staples Lewis           | Christian life               | http://books.google.com/books/content?id=Kk-uV...                         | "In The Problem of Pain, C.S. Lewis, one of th...                           | 2002.0         | 4.09           | 176       | The Problem of Pain    | 9780006280934 "In The Problem of Pain, C.S. Le...                               | Nonfiction       | 0.081412 | 0.184495 | 0.095043 | 0.040564 | 0.884390 | 0.475881 | 0.078765 |


## Gradio Dashboard:
To make the recommender interactive and user-friendly, a Gradio dashboard was created. The interface allows users to provide three inputs:
- Book Description (Query): A short description of what the user is looking for.
- Category: One of the predefined categories – Fiction, Nonfiction, Children’s Fiction, Children’s Nonfiction – or All.
- Emotional Tone: One of the emotions – anger, disgust, fear, joy, sadness, surprise, neutral – or All.

Based on these inputs, the recommender returns the top 16 book results, displaying:
- Book covers
- Titles
- Authors
- Descriptions

<img width="1919" height="935" alt="Gradio Dashboard Screenshot" src="https://github.com/user-attachments/assets/33a23d7c-1e3d-433a-a763-a52f99365c63" />
