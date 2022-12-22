# Exploratory data analysis of the Walt Disney company datasets


#### Student: Rawan Alghamdi[¶]


Foreword
------------------------------------

In this notebook, I will do some exploratory data analysis for the
`Disney` dataset[1](https://data.world/kgarrett/disney-character-success-00-16). I will
be using the tables provided to answer questions that could be of
interest to several sectors and businesses.

Introduction
--------------------------------------------------------------------


Question(s) of interests
--------------------------------------------------------------------

In this analysis, I will be working with the collection of the `Disney`
dataset to explore questions that could be used in several sectors,
mainly businesses investing in Disney\'s products and services.

I am interested in finding out the most popular genre and the most loved
Disney hero in each genre based on their gross revenue. This is
interesting and beneficial to know, especially for toy shops, souvenir
shops and other places that want to increase their sales. I am expecting
the **Musical** genre to be the most popular since Disney is knowns for
having many hits songs. And **Elsa** from the movie Frozen to be the
most famous hero.


Dataset description
----------------------------------------------------------

The Walt Disney Company is a multimedia entertainment company based in
Burbank, California and identifies as one of the biggest and most
well-known companies in the world. It has many divisions, such as Disney
Parks and Products, Disney Media and Entertainment Distribution, The
Walt Disney Studios and many more
[2](https://thewaltdisneycompany.com/about/#our-businesses).

The Disney dataset is composed of \$5\$ tables, `disney-characters.csv`,
`disney-director.csv`, `disney-voice-actors.csv`,
`disney_revenue_1991-2016.csv`, and `disney_movies_total_gross.csv`.
Each table is stored in the `.csv` format and contain different
information about different divisions such as Disney characters,
directors, songs and voice actors, total gross and more. For this
analysis and to answer the proposed questions, I will be using three
tables as follow:

-   **disney-characters.csv**

    > This file contains information on Disney movies including the
    > movie titles, the release date, the hero, the villain and the main
    > song of the movie.

-   **disney\_movies\_total\_gross.csv**

    > This file includes information on Disney movies including movie
    > titles, the release date, the movie genre, the MPAA rating(which
    > are either G, PG, PG-13, R or Non), the total gross revenue and
    > the total gross after inflation.

-   **disney\_revenue\_1991-2016.csv**

    > This file includes information on Disney revenue including the
    > year from 1991 to 2016, the Studio Entertainment revenue, the
    > Disney Consumer Products revenue, the Disney Interactive revenue,
    > the Walt Disney Parks and Resorts revenue, the Disney Media
    > Networks revenue and the total revenue for each year.


Methods and Results
==========================================================

Since I am interested in finding the most popular genre of movies and
heroes from each movie. I will be using both the total gross revenue and
total gross post-inflation from the table `disney_movies_total_gross`.

I will start by importing the tables and the libraries I will use for
the analysis. Next, I will clean the data by removing unnecessary
characters from the `disney-characters` data.

```
    # Import the required libraries needed for this analysis
    import altair as alt
    import pandas as pd

    # Import all the required files
    disney_characters = pd.read_csv(
        "data/disney-characters.csv", parse_dates=["release_date"]
    )
    disney_movies_total_gross = pd.read_csv(
        "data/disney_movies_total_gross.csv", parse_dates=["release_date"]
    )
    disney_revenue = pd.read_csv("data/disney_revenue_1991-2016.csv")
 ```

```
    # Clean the column 'movie_title' in the data characters
    characters = disney_characters.assign(
        movie_title=disney_characters["movie_title"].str.strip("\n")
    )
```
I will check what the characters table looks like and get some other
information about it.

```characters.head()
```

```
      movie\_title                      release\_date   hero         villian      song
  --- --------------------------------- --------------- ------------ ------------ ------------------------------
  0   Snow White and the Seven Dwarfs   1937-12-21      Snow White   Evil Queen   Some Day My Prince Will Come
  1   Pinocchio                         1940-02-07      Pinocchio    Stromboli    When You Wish upon a Star
  2   Fantasia                          1940-11-13      NaN          Chernabog    NaN
  3   Dumbo                             1941-10-23      Dumbo        Ringmaster   Baby Mine
  4   Bambi                             1942-08-13      Bambi        Hunter       Love Is a Song
```

```characters.info()
    ```
    
```Data columns (total 5 columns):
     #   Column        Non-Null Count  Dtype         
    ---  ------        --------------  -----         
     0   movie_title   56 non-null     object        
     1   release_date  56 non-null     datetime64[ns]
     2   hero          52 non-null     object        
     3   villian       46 non-null     object        
     4   song          47 non-null     object        
    dtypes: datetime64[ns](1), object(4)
    memory usage: 2.3+ KB
    ```








::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
The `characters` table has \$56 \$ rows and \$5\$ columns. Every
**movie\_title** has a **release\_date**, a **hero**, a **villian** and
a **song**. However, we can see that some movies do not have a **hero**,
a **villian** and a **song**. I will later drop the NaN values in the
column **hero** which I will be using in this analysis.

Next, I will check what the `movies_total_gross` table looks like and
get some other information about it.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[5\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    disney_movies_total_gross.head()
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
Out\[5\]:
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output .jp-OutputArea-executeResult mime-type="text/html"}
<div>

      movie\_title                      release\_date   genre       MPAA\_rating   total\_gross    inflation\_adjusted\_gross
  --- --------------------------------- --------------- ----------- -------------- --------------- ----------------------------
  0   Snow White and the Seven Dwarfs   1937-12-21      Musical     G              \$184,925,485   \$5,228,953,251
  1   Pinocchio                         1940-02-09      Adventure   G              \$84,300,000    \$2,188,229,052
  2   Fantasia                          1940-11-13      Musical     G              \$83,320,000    \$2,187,090,808
  3   Song of the South                 1946-11-12      Adventure   G              \$65,000,000    \$1,078,510,579
  4   Cinderella                        1950-02-15      Drama       G              \$85,000,000    \$920,608,730

</div>
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[6\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    disney_movies_total_gross.info()
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 579 entries, 0 to 578
    Data columns (total 6 columns):
     #   Column                    Non-Null Count  Dtype         
    ---  ------                    --------------  -----         
     0   movie_title               579 non-null    object        
     1   release_date              579 non-null    datetime64[ns]
     2   genre                     562 non-null    object        
     3   MPAA_rating               523 non-null    object        
     4   total_gross               579 non-null    object        
     5   inflation_adjusted_gross  579 non-null    object        
    dtypes: datetime64[ns](1), object(5)
    memory usage: 27.3+ KB
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
The `disney_movies_total_gross` table has \$579\$ rows and \$6\$
columns. Every **movie\_title** has a **release\_date**, a **genre**, a
**MPAA\_rating**, a **total\_gross** and an
**inflation\_adjusted\_gross** value. However, we can see that some
movies do not have a categorized **genre** or **MPAA\_rating**.

I will now clean unnecessary characters from the columns
**total\_gross** and **inflation\_adjusted\_gross** to be able to adjust
their dtype to integers.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell .jp-mod-noOutputs}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[7\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    # Clean the columns 'inflation_adjusted_gross' and 'total_gross' in the data movies_total_gross
    movies_total_gross = disney_movies_total_gross.assign(
        inflation_adjusted_gross=disney_movies_total_gross[
            "inflation_adjusted_gross"
        ].str.strip("$"),
        total_gross=disney_movies_total_gross["total_gross"].str.strip("$"),
    )

    movies_total_gross = movies_total_gross.assign(
        inflation_adjusted_gross=movies_total_gross["inflation_adjusted_gross"].str.replace(
            ",", ""
        ),
        total_gross=movies_total_gross["total_gross"].str.replace(",", ""),
    )
    # Change the type to an integer
    movies_total_gross = movies_total_gross.astype(
        {"inflation_adjusted_gross": int, "total_gross": int}
    )
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
Lastly, I will check what the `disney_revenue` table looks like and get
some other information about it
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[8\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    disney_revenue.head()
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
Out\[8\]:
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output .jp-OutputArea-executeResult mime-type="text/html"}
<div>

      Year   Studio Entertainment\[NI 1\]   Disney Consumer Products\[NI 2\]   Disney Interactive\[NI 3\]\[Rev 1\]   Walt Disney Parks and Resorts   Disney Media Networks   Total
  --- ------ ------------------------------ ---------------------------------- ------------------------------------- ------------------------------- ----------------------- -------
  0   1991   2593.0                         724.0                              NaN                                   2794.0                          NaN                     6111
  1   1992   3115.0                         1081.0                             NaN                                   3306.0                          NaN                     7502
  2   1993   3673.4                         1415.1                             NaN                                   3440.7                          NaN                     8529
  3   1994   4793.0                         1798.2                             NaN                                   3463.6                          359                     10414
  4   1995   6001.5                         2150.0                             NaN                                   3959.8                          414                     12525

</div>
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[9\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    disney_revenue.info()
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 26 entries, 0 to 25
    Data columns (total 7 columns):
     #   Column                           Non-Null Count  Dtype  
    ---  ------                           --------------  -----  
     0   Year                             26 non-null     int64  
     1   Studio Entertainment[NI 1]       25 non-null     float64
     2   Disney Consumer Products[NI 2]   24 non-null     float64
     3   Disney Interactive[NI 3][Rev 1]  12 non-null     float64
     4   Walt Disney Parks and Resorts    26 non-null     float64
     5   Disney Media Networks            23 non-null     object 
     6   Total                            26 non-null     int64  
    dtypes: float64(4), int64(2), object(1)
    memory usage: 1.5+ KB
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
The `disney_revenue` table has \$26\$ rows and \$7\$ columns. Every
**year** has a **Studio Entertainment**, a **Disney Consumer Products**,
a **Disney Interactive**, a **Walt Disney Parks and Resorts**, a
**Disney Media Networks** and **Total** value. We can see many
missing/NaN values in the table. We will lose a big portion of the table
if we were to drop the missing values. Hence, I will only use the
**Total** column for this analysis since it is incomplete.

Next, I will import and use the custom-made function that takes in a
data frame and groups it by a certain column and then takes the average
revenue. We can use gross revenue, net revenue or revenue after
inflation. The function also optionally returns a bar plot to visualize
the data.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell .jp-mod-noOutputs}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[10\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    # Importing the function average_revenue
    from average_revenue import average_revenue
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
For the first visulasiation, I want to know the most poular genra based
on the average of total gross.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[11\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    popular_genre = average_revenue(movies_total_gross, "genre", "total_gross", "show")
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    Adventure in genre have the highest avergae total_gross of 127047050.02325581
    Documentary in genre have the lowest avergae total_gross of 11292851.1875
:::
:::

::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output mime-type="text/html"}
::: {#altair-viz-6a1df4bd1bc1464b8642894bfec6b473}
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
It turns out that Adventure has the highest average total gross followed
by Action and Musical genres.

Next, I will plot the second visualization using the total adjusted
gross after inflation to explore how inflation affected the total gross
over the years.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[12\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    popular_genre_adjusted = average_revenue(
        movies_total_gross, "genre", "inflation_adjusted_gross", "show"
    )
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    Musical in genre have the highest avergae inflation_adjusted_gross of 603597861.0
    Documentary in genre have the lowest avergae inflation_adjusted_gross of 12718026.125
:::
:::

::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output mime-type="text/html"}
::: {#altair-viz-ec4d64bab5a6455ba529528d40dfeef9}
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
After inflation, It turns out that Musical has the highest average total
gross after inflation followed by Aventure and Action genres. There is a
drastic difference between the Musical and Adventure genres in the
second visualization. Hence, the Musical genre is the most popular one
based on the adjusted total gross after inflation as we expected.

Based on the House Committee on Oversight and Reform, economic studies
and the Subcommittee's analysis showed that industries show massive
increases in profit after inflation. Therefore, we will base our results
using adjusted total gross for this analysis
[3](https://oversight.house.gov/news/press-releases/subcommittee-analysis-reveals-excessive-corporate-price-hikes-have-hurt).

The high total gross for Adventure and action genres can be explained by
the high production cost for these genes, which does not necessarily
mean a high profit (net revenue) from these genes.
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
To answer the second question about the most famous Disney hero, I will
merge the two table `characters` and `movies_total_gross` to investigate
which hero made the highest gross revenue.

I will start first by dropping the missing value from the **hero**
column which we explored earlier in this analysis. And I will also drop
the **release\_date** so we don\'t have repetitive columns.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell .jp-mod-noOutputs}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[13\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    # Extract rows with out NAN values in the 'hero' column in characters dataframe using the drop.na() function
    characters_hero = characters.dropna(subset=["hero"]).drop(columns="release_date")
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
I will now merge the two tables on the **movie\_title** column.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[14\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    # Reset the index so we can plot using altair in the average_revenue function
    movies_hero = pd.merge(
        characters_hero, movies_total_gross, on="movie_title", how="inner"
    )  # .reset_index()
    movies_hero.info()
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 46 entries, 0 to 45
    Data columns (total 9 columns):
     #   Column                    Non-Null Count  Dtype         
    ---  ------                    --------------  -----         
     0   movie_title               46 non-null     object        
     1   hero                      46 non-null     object        
     2   villian                   41 non-null     object        
     3   song                      42 non-null     object        
     4   release_date              46 non-null     datetime64[ns]
     5   genre                     45 non-null     object        
     6   MPAA_rating               39 non-null     object        
     7   total_gross               46 non-null     int64         
     8   inflation_adjusted_gross  46 non-null     int64         
    dtypes: datetime64[ns](1), int64(2), object(6)
    memory usage: 3.6+ KB
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
Next, I will reuse the same function to find out the most famous Disney
hero based on the total gross revenue.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[15\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    popular_hero = average_revenue(movies_hero, "hero", "total_gross", "show")
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    Simba in hero have the highest avergae total_gross of 422780140.0
    Aurora in hero have the lowest avergae total_gross of 9464608.0
:::
:::

::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output mime-type="text/html"}
::: {#altair-viz-69c65e7a6864420faf85d6dfee9d43c7}
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
The plot shows that Simba is the most famous hero followed by Elsa. We
can see a minor difference in total gross between the heroes.

I will now plot the second visualization using the adjusted total gross
after inflation to explore more.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[16\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    popular_hero_adjusted = average_revenue(
        movies_hero, "hero", "inflation_adjusted_gross", "show"
    )
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedText .jp-OutputArea-output mime-type="text/plain"}
    Snow White in hero have the highest avergae inflation_adjusted_gross of 5228953251.0
    Winnie the Pooh in hero have the lowest avergae inflation_adjusted_gross of 14187934.5
:::
:::

::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output mime-type="text/html"}
::: {#altair-viz-d88a4b8ce68c4607b4dab5ce99adfcf9}
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
After inflation, it turns out that Snow White is the most famous Disney
hero with a major difference compared to other heroes. These results
contradict our prediction, but they can be explained by the publication
date of the character. We know that the Seven Dwarfs movie have been
published in 1937 which gave the character a long time to make huge
commercial success internationally compared to Elsa or Simba. Also, we
need to take into consideration that the Snow White and the Seven Dwarfs
story have been improved and republished many times over the years.

To conclude, we will base our results using adjusted total gross for
this analysis. And we find out that the most popular Disney genre is
Musical and the most famous character is Snow White from the Snow White
and the Seven Dwarfs movie. However, we still don\'t know exactly how
much net revenue or profit each genre or hero made.

I will use the `disney_revenue` table to explore the total Disney
revenue using chart visualization.
:::
:::
:::
:::

::: {.jp-Cell .jp-CodeCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
In \[17\]:
:::

::: {.jp-CodeMirrorEditor .jp-Editor .jp-InputArea-editor data-type="inline"}
::: {.CodeMirror .cm-s-jupyter}
::: {.highlight .hl-ipython3}
    # plot revenue using altair
    yearly_revenue = (
        alt.Chart(disney_revenue, width=500, height=300)
        .mark_circle(color="red", size=80, opacity=1)
        .encode(
            x=alt.X("Year:O", sort="y", title="Year"),
            y=alt.Y("Total:Q", title="Total Revenue"),
        )
        .properties(title="Total Revenue of Disney By year")
    ).interactive()
    yearly_revenue
:::
:::
:::
:::
:::

::: {.jp-Cell-outputWrapper}
::: {.jp-Collapser .jp-OutputCollapser .jp-Cell-outputCollapser}
:::

::: {.jp-OutputArea .jp-Cell-outputArea}
::: {.jp-OutputArea-child}
::: {.jp-OutputPrompt .jp-OutputArea-prompt}
Out\[17\]:
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedHTML .jp-OutputArea-output .jp-OutputArea-executeResult mime-type="text/html"}
::: {#altair-viz-d550fc455ffb4326a4d675171abba14c}
:::
:::
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
We can clearly see the linear increase in the total Disney revenue over
the years. This chart alines with the idea that old characters would
increase in revenue if have been developed and republished. The economic
and inflation factors also played a role in the yearly revenue increase.
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
Discussions[¶](#Discussions){.anchor-link} {#Discussions}
==========================================

In this notebook, I analyzed the Disney datasets and computed the most
popular Disney genre and hero. Before reaching the answer, I applied a
custom-made function that helped visualize the data. I used the total
gross and the adjusted total gross after inflation to explore which
genre and hero has the highest average gross revenue. Based on House
Committee on Oversight and Reformthe, I relied on the average of the
adjusted total gross after inflation to derive the results. I found out
that the Musical genre has the highest average adjusted total gross
which matched my expectation. And I found out that the Snow white had an
extremely high total gross average after inflation which was not
expected.

My original guess for the most famous hero would have been Elsa from the
movie Frozen. I was amazed to know that Snow White and the Seven Dwarfs
movie have been published in 1937 and has been recreated and republished
several times by the Walt Disney Studios. Comparing this old character
to a new character developed recently, the results aline with the
hero\'s character background and success.

However, the order for the rest of the genre and hero computed might not
be accurate. For example in the second plot, we can see that Comedy,
Romantic Comedy, Western and Drama genres have almost no difference in
total gross. The same for the heroes Aladdin, Elsa, Mowgli and Belle in
the fourth plot. Therefore, we need to ask another question for more
accurate results: What is the net revenue for each genre and each Disney
movie? Since we know that gross revenue includes production cost,
payroll net pay and others. Net revenue is the profit Disney made from a
movie or genre. It will be interesting to know if old heroes are still
making a high profit or if they will be abundant over the years.
:::
:::
:::
:::

::: {.jp-Cell .jp-MarkdownCell .jp-Notebook-cell}
::: {.jp-Cell-inputWrapper}
::: {.jp-Collapser .jp-InputCollapser .jp-Cell-inputCollapser}
:::

::: {.jp-InputArea .jp-Cell-inputArea}
::: {.jp-InputPrompt .jp-InputArea-prompt}
:::

::: {.jp-RenderedHTMLCommon .jp-RenderedMarkdown .jp-MarkdownOutput mime-type="text/markdown"}
References[¶](#References){.anchor-link} {#References}
========================================

-   1.  [Disney Data
        Source](https://data.world/kgarrett/disney-character-success-00-16)
        -   This Disney database used in this work.

-   1.  [The Walt Disney
        Company](https://thewaltdisneycompany.com/about/#our-businesses)
        -   The website used to describe the Disney dataset background.

-   1.  [House Committee on Oversight and
        Reform](https://oversight.house.gov/news/press-releases/subcommittee-analysis-reveals-excessive-corporate-price-hikes-have-hurt)
        -   The website used to identify the impact of inflation on
            industries.

-   1.  [Film Reviews: Snow White and 7
        Dwarfs](https://archive.org/details/variety128-1937-12/page/n265/mode/2up)
        -   The Snow White and 7 Dwarfs movie has been published in
            1937, according to the Variety Publishing Company.
:::
:::
:::
:::
