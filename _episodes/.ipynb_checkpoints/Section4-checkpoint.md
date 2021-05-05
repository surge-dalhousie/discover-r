## Data Cleaning & Tranformation




```R
library(fs)
```

### Load the `tidyverse` and data frame into your R session


```R
library(tidyverse)
```

    Warning message in system("timedatectl", intern = TRUE):
    “running command 'timedatectl' had status 1”
    ── [1mAttaching packages[22m ─────────────────────────────────────── tidyverse 1.3.1 ──
    
    [32m✔[39m [34mggplot2[39m 3.3.3     [32m✔[39m [34mpurrr  [39m 0.3.4
    [32m✔[39m [34mtibble [39m 3.1.1     [32m✔[39m [34mdplyr  [39m 1.0.5
    [32m✔[39m [34mtidyr  [39m 1.1.3     [32m✔[39m [34mstringr[39m 1.4.0
    [32m✔[39m [34mreadr  [39m 1.4.0     [32m✔[39m [34mforcats[39m 0.5.1
    
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    



```R
books <- read_csv("data/books.csv")  # load the data and assign it to books
```

    
    [36m──[39m [1m[1mColumn specification[1m[22m [36m────────────────────────────────────────────────────────[39m
    cols(
      CALL...BIBLIO. = [31mcol_character()[39m,
      X245.ab = [31mcol_character()[39m,
      X245.c = [31mcol_character()[39m,
      LOCATION = [31mcol_character()[39m,
      TOT.CHKOUT = [32mcol_double()[39m,
      LOUTDATE = [31mcol_character()[39m,
      SUBJECT = [31mcol_character()[39m,
      ISN = [31mcol_character()[39m,
      CALL...ITEM. = [31mcol_character()[39m,
      X008.Date.One = [31mcol_character()[39m,
      BCODE2 = [31mcol_character()[39m,
      BCODE1 = [31mcol_character()[39m
    )
    
    


### Transforming data with `dplyr`

We are now entering the data cleaning and transforming phase. While it is possible to do much of the following using Base R functions (in other words, without loading an external package) `dplyr` makes it much easier. Like many of the most useful R packages, `dplyr` was developed by data scientist http://hadley.nz/.

`dplyr` is a package for making tabular data manipulation easier by using a limited set of functions that can be combined to extract and summarize insights from your data. It pairs nicely with `tidyr` which enables you to swiftly convert between different data formats (long vs. wide) for plotting and analysis.

`dplyr` is also part of the `tidyverse`. Let’s make sure we are all on the same page by loading the `tidyverse` and the `books` dataset we downloaded earlier.

We’re going to learn some of the most common `dplyr` functions:

- `rename()`: rename columns
- `recode()`: recode values in a column
- `select()`: subset columns
- `filter()`: subset rows on conditions
- `mutate()`: create new columns by using information from other columns
- `group_by()` and `summarize()`: create summary statistics on grouped data
- `arrange()`: sort results
- `count()`: count discrete values

### Renaming variables

It is often necessary to rename variables to make them more meaningful. If you print the names of the sample `books` dataset you can see that some of the vector names are not particularly helpful:


```R
glimpse(books)
```

    Rows: 10,000
    Columns: 12
    $ CALL...BIBLIO. [3m[90m<chr>[39m[23m "001.94 Don 2000", "001.942 Bro 1999", "027.073 App 200…
    $ X245.ab        [3m[90m<chr>[39m[23m "Bermuda Triangle /", "Invaders from outer space :|real…
    $ X245.c         [3m[90m<chr>[39m[23m "written by Andrew Donkin.", "written by Philip Brooks.…
    $ LOCATION       [3m[90m<chr>[39m[23m "juv", "juv", "juv", "juv", "juv", "juv", "juv", "juv",…
    $ TOT.CHKOUT     [3m[90m<dbl>[39m[23m 6, 2, 3, 6, 7, 6, 4, 2, 4, 13, 6, 7, 3, 22, 2, 9, 4, 8,…
    $ LOUTDATE       [3m[90m<chr>[39m[23m "11-21-2013 9:44", "02-07-2004 15:29", "10-16-2007 10:5…
    $ SUBJECT        [3m[90m<chr>[39m[23m "Readers (Elementary)|Bermuda Triangle -- Juvenile lite…
    $ ISN            [3m[90m<chr>[39m[23m "0789454165 (hbk.)~0789454157 (pbk.)", "0789439999 (har…
    $ CALL...ITEM.   [3m[90m<chr>[39m[23m "001.94 Don 2000", "001.942 Bro 1999", "027.073 App 200…
    $ X008.Date.One  [3m[90m<chr>[39m[23m "2000", "1999", "2001", "1999", "2000", "2001", "2001",…
    $ BCODE2         [3m[90m<chr>[39m[23m "a", "a", "a", "a", "a", "a", "a", "a", "a", "a", "a", …
    $ BCODE1         [3m[90m<chr>[39m[23m "j", "j", "j", "j", "j", "j", "j", "j", "j", "j", "j", …


There are many ways to rename variables in R, but the `rename()` function in the `dplyr` package is the easiest and most straightforward. The new variable name comes first. See `help(rename)`.

Here we rename the X245.ab variable. Make sure you assign the output to your `books` value, otherwise it will just print it to the console. In other words, we are overwritting the previous `books` value with the new one, with `X245.ab` changed to `title`


```R
# rename the . Make sure you return (<-) the output to your 
# variable, otherwise it will just print it to the console
books <- rename(books,
                title = X245.ab)
```

#### Side note:

Where does `X245.ab` come from? That is the MARC field 245|ab. However, because R variables cannot start with a number, R automatically inserted and X, and because pipes | are not allowed in variable names, R replaced it with a period.


```R
# rename multiple variables at once
books <- rename(books,
                author = X245.c,
                callnumber = CALL...BIBLIO.,
                isbn = ISN,
                pubyear = X008.Date.One,
                subCollection = BCODE1,
                format = BCODE2,
                location = LOCATION,
                tot_chkout = TOT.CHKOUT,
                loutdate = LOUTDATE,
                subject = SUBJECT)
books
```

    WARNING: Some output was deleted.


#### Exercise: Rename `CALL...ITEM.`

- Use rename() to rename the CALL...ITEM. column to callnumber2. Remember to add the period to the end of the CALL...ITEM. value


```R

```

### Recoding values

It is often necessary to recode or reclassify values in your data. For example, in the sample dataset provided to you, the `sub_collection` (formerly `BCODE1`) and `format` (formerly `BCODE2`) variables contain single characters. You can do this easily using the `recode()` function, also in the `dplyr` package. Unlike `rename()`, the old value comes first here. Also notice that we are overwriting the `books$subCollection` variable.


```R
# first print to the console all of the unique values you will need to recode
distinct(books, subCollection)
```




<table class="dataframe">
<caption>A tibble: 10 × 1</caption>
<thead>
	<tr><th scope=col>subCollection</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>j</td></tr>
	<tr><td>b</td></tr>
	<tr><td>u</td></tr>
	<tr><td>r</td></tr>
	<tr><td>-</td></tr>
	<tr><td>s</td></tr>
	<tr><td>c</td></tr>
	<tr><td>z</td></tr>
	<tr><td>a</td></tr>
	<tr><td>t</td></tr>
</tbody>
</table>





```R
books$subCollection <- recode(books$subCollection,
                                      "-" = "general collection",
                                      u = "government documents",
                                      r = "reference",
                                      b = "k-12 materials",
                                      j = "juvenile",
                                      s = "special collections",
                                      c = "computer files",
                                      t = "theses",
                                      a = "archives",
                                      z = "reserves")
books
```

    WARNING: Some output was deleted.


Do the same for the `format` column. Note that you must put `"5"` and `"4"` into quotation marks for the function to operate correctly.


```R
books$format <- recode(books$format,
                              a = "book",
                              e = "serial",
                              w = "microform",
                              s = "e-gov doc",
                              o = "map",
                              n = "database",
                              k = "cd-rom",
                              m = "image",
                              "5" = "kit/object",
                              "4" = "online video")
```

### Subsetting dataframes

#### Subsetting using `filter()` in the `dplyr` package

In the last lesson we learned how to subset a data frame using brackets. As with other R functions, the `dplyr` package makes it much more straightforward, using the `filter()` function.

Here we will create a subset of `books` called `booksOnly`, which includes only those items where the format is books. Notice that we use two equal signs `==` as the logical operator:


```R
booksOnly <- filter(books, format == "book") # filter books to return only those items where the format is books
```

You can also use multiple filter conditions. Here, the order matters: first we filter to include only books, then of the results, we include only items that have more than zero checkouts.


```R
bookCheckouts <- filter(books,
                        format == "book",
                        tot_chkout > 0)
```

How many items were removed? You can find out functionally with:


```R
nrow(books) - nrow(bookCheckouts)
```




5733



You can then check the summary statistics of checkouts for books with more than zero checkouts. Notice how different these numbers are from the previous lesson, when we kept zero in. The median is now 3 and the mean is 5.


```R
summary(bookCheckouts$tot_chkout)
```




       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
      1.000   2.000   3.000   5.281   6.000 113.000 



If you want to filter on multiple conditions within the same variable, use the `%in%` operator combined with a vector of all the values you wish to include within `c()`. For example, you may want to include only items in the format `serial` and `microform`:


```R
serial_microform <- filter(books, format %in% c("serial", "microform"))
```

#### Exercise: Filtering with `filter()`
- Use `filter()` to create a data frame called `booksJuv` consisting of `format` books and `subCollection` juvenile materials.
- Use `mean()` to check the average number of checkouts for the `booksJuv` data frame

### Selecting variables

The `select()` function allows you to keep or remove specific columns. It also provides a convenient way to reorder variables.


```R
# specify the variables you want to keep by name
booksTitleCheckouts <- select(books, title, tot_chkout)
booksTitleCheckouts
```




<table class="dataframe">
<caption>A tibble: 10000 × 2</caption>
<thead>
	<tr><th scope=col>title</th><th scope=col>tot_chkout</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Bermuda Triangle /                                                                            </td><td> 6</td></tr>
	<tr><td>Invaders from outer space :|real-life stories of UFOs /                                       </td><td> 2</td></tr>
	<tr><td>Down Cut Shin Creek :|the pack horse librarians of Kentucky /                                 </td><td> 3</td></tr>
	<tr><td>The Chinese book of animal powers /                                                           </td><td> 6</td></tr>
	<tr><td>Judge Judy Sheindlin's Win or lose by how you choose! /                                       </td><td> 7</td></tr>
	<tr><td>Judge Judy Sheindlin's You can't judge a book by its cover :|cool rules for school /          </td><td> 6</td></tr>
	<tr><td>A young child's Bible /                                                                       </td><td> 4</td></tr>
	<tr><td>God's Kingdom :|stories from the New Testament /                                              </td><td> 2</td></tr>
	<tr><td>Roman myths /                                                                                 </td><td> 4</td></tr>
	<tr><td>Greek gods and goddesses /                                                                    </td><td>13</td></tr>
	<tr><td>The story of Shabbat /                                                                        </td><td> 6</td></tr>
	<tr><td>On Shabbat /                                                                                  </td><td> 7</td></tr>
	<tr><td>On Purim /                                                                                    </td><td> 3</td></tr>
	<tr><td>Coming to America :|the story of immigration /                                                </td><td>22</td></tr>
	<tr><td>The shared heart :|portraits and stories celebrating lesbian, gay, and bisexual young people /</td><td> 2</td></tr>
	<tr><td>Remember the ladies :|100 great American women /                                              </td><td> 9</td></tr>
	<tr><td>One Belfast boy /                                                                             </td><td> 4</td></tr>
	<tr><td>One boy from Kosovo /                                                                         </td><td> 8</td></tr>
	<tr><td>Slavery :|bondage throughout history /                                                        </td><td> 3</td></tr>
	<tr><td>River town /                                                                                  </td><td> 5</td></tr>
	<tr><td>Kids on strike! /                                                                             </td><td> 9</td></tr>
	<tr><td>A kids' guide to America's Bill of Rights :|curfews, censorship, and the 100-pound giant /    </td><td> 4</td></tr>
	<tr><td>Do you remember the color blue? :|and other questions kids ask about blindness /              </td><td> 5</td></tr>
	<tr><td>The wildlife detectives :|how forensic scientists fight crimes against nature /               </td><td> 2</td></tr>
	<tr><td>The environmental movement :|from its roots to the challenges of a new century /              </td><td> 3</td></tr>
	<tr><td>After the spill :|the Exxon Valdez disaster, then and now /                                   </td><td> 5</td></tr>
	<tr><td>Juvenile crime /                                                                              </td><td> 4</td></tr>
	<tr><td>One-room school /                                                                             </td><td> 6</td></tr>
	<tr><td>Reader's choice :|the books children love to read and teachers love to teach.                 </td><td> 1</td></tr>
	<tr><td>Reader's choice :|the books children love to read and teachers love to teach.                 </td><td> 7</td></tr>
	<tr><td>⋮</td><td>⋮</td></tr>
	<tr><td>Lectura Scott Foresman /                                                                        </td><td> 4</td></tr>
	<tr><td>Employment dispute resolution and worker rights in the changing workplace /                     </td><td> 2</td></tr>
	<tr><td>The best teacher "stuff" /                                                                      </td><td> 8</td></tr>
	<tr><td>Rosso in Italy :|the Italian career of Rosso Fiorentino /                                       </td><td> 0</td></tr>
	<tr><td>CL locker keys                                                                                  </td><td>13</td></tr>
	<tr><td>Many peoples, one land :|a guide to new multicultural literature for children and young adults /</td><td> 0</td></tr>
	<tr><td>Conceptual revolutions /                                                                        </td><td> 0</td></tr>
	<tr><td>The journals of gerontology.                                                                    </td><td> 0</td></tr>
	<tr><td>U.S. master tax guide.                                                                          </td><td> 0</td></tr>
	<tr><td>Central business review.                                                                        </td><td> 0</td></tr>
	<tr><td>Steamboat! :|the story of Captain Blanche Leathers /                                            </td><td> 5</td></tr>
	<tr><td>Collections.|a Harcourt reading/language arts program.                                          </td><td>20</td></tr>
	<tr><td>Co-operation in international business :|the use of limited equity arrangements /               </td><td> 0</td></tr>
	<tr><td>McGraw-Hill reading.                                                                            </td><td> 4</td></tr>
	<tr><td>McGraw-Hill lectura /                                                                           </td><td> 2</td></tr>
	<tr><td>Collections :|a Harcourt reading program /                                                      </td><td> 0</td></tr>
	<tr><td>Voz del lector :|los libros que los ninos quieren leer Y los maestros quieren ensenar.          </td><td> 7</td></tr>
	<tr><td>Early supplementary feeding and cognition :|effects over two decades /                          </td><td> 0</td></tr>
	<tr><td>Collections :|a Harcourt reading/language arts program /                                        </td><td>18</td></tr>
	<tr><td>Lectura Scott Foresman /                                                                        </td><td> 3</td></tr>
	<tr><td>McGraw-Hill lectura /                                                                           </td><td> 1</td></tr>
	<tr><td>McGraw-Hill lectura /                                                                           </td><td> 1</td></tr>
	<tr><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                           </td><td> 0</td></tr>
	<tr><td>Scott Foresman reading /                                                                        </td><td> 4</td></tr>
	<tr><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                           </td><td> 6</td></tr>
	<tr><td>Population today.                                                                               </td><td> 0</td></tr>
	<tr><td>Practical plays /                                                                               </td><td> 3</td></tr>
	<tr><td><span style=white-space:pre-wrap>Science, technology &amp; human values.                                                             </span></td><td> 0</td></tr>
	<tr><td>CL locker keys                                                                                  </td><td>13</td></tr>
	<tr><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                           </td><td> 2</td></tr>
</tbody>
</table>





```R
# specify the variables you want to remove with a -
books <- select(books, -location)

# reorder columns, combined with everything()
booksReordered <- select(books, title, tot_chkout, loutdate, everything())
```

### Ordering data

The `arrange()` function in the `dplyr` package allows you to sort your data by alphabetical or numerical order


```R
booksTitleArrange <- arrange(books, title)

# use desc() to sort a variable in descending order
booksHighestChkout <- arrange(books, desc(tot_chkout))
booksHighestChkout
```

    WARNING: Some output was deleted.



```R
# order data based on multiple variables (e.g. sort first by checkout, then by publication year)
booksChkoutYear <- arrange(books, desc(tot_chkout), desc(pubyear))
```

### Creating new variables

The `mutate()` function allows you to create new variables. Here, we use the `str_sub()` function from the `stringr` package to extract the first character of the `callnumber` variable (the call number class) and put it into a new column called `call_class`.


```R
booksLC <- mutate(books,
                  call_class = str_sub(callnumber, 1, 1))
```

There are two numbers because you must specify a start and an end value–here, we start with the first character, and end with the first character.

`mutate()` is also helpful to coerce a column from one data type to another. For example, we can see there are some errors in the `pubyear` variable–some dates are `19zz` or `uuuu`. As a result, this variable was read in as a `character` rather than an `integer`.


```R
books <- mutate(books, pubyear = as.integer(pubyear))
```

    Warning message in mask$eval_all_mutate(quo):
    “NAs introduced by coercion”


We see the error message `NAs introduced by coercion`. This is because non-numerical variables become `NA` and the remainder become integers.

### Pattern matching

Cleaning text with the `stringr` package is easier when you have a basic understanding of ‘regex’, or regular expression pattern matching. Regex is especially useful for manipulating strings (alphanumeric data), and is the backbone of search-and-replace operations in most applications. Pattern matching is common to all programming languages but regex syntax is often code-language specific. Below, find an example of using pattern matching to find and replace data in R:

1. Remove the trailing slash in the title column
2. Modify the punctuation separating the title from a subtitle

Note: If the final product of this data will be imported into an ILS, you may not want to alter the MARC specific punctuation. All other audiences will appreciate the text normalizing steps.

Read more about [matching patterns with regular expressions](https://r4ds.had.co.nz/strings.html#matching-patterns-with-regular-expressions).


```R
books %>% 
  mutate(title_modified = str_remove(title, "/$")) %>%     # remove the trailing slash
  mutate(title_modified = str_replace(title_modified, "\\s:\\|", ": ")) %>%   # replace ' :|' with ': '
  select(title_modified, title)
```




<table class="dataframe">
<caption>A tibble: 10000 × 2</caption>
<thead>
	<tr><th scope=col>title_modified</th><th scope=col>title</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Bermuda Triangle                                                                            </td><td>Bermuda Triangle /                                                                            </td></tr>
	<tr><td>Invaders from outer space: real-life stories of UFOs                                        </td><td>Invaders from outer space :|real-life stories of UFOs /                                       </td></tr>
	<tr><td>Down Cut Shin Creek: the pack horse librarians of Kentucky                                  </td><td>Down Cut Shin Creek :|the pack horse librarians of Kentucky /                                 </td></tr>
	<tr><td>The Chinese book of animal powers                                                           </td><td>The Chinese book of animal powers /                                                           </td></tr>
	<tr><td>Judge Judy Sheindlin's Win or lose by how you choose!                                       </td><td>Judge Judy Sheindlin's Win or lose by how you choose! /                                       </td></tr>
	<tr><td>Judge Judy Sheindlin's You can't judge a book by its cover: cool rules for school           </td><td>Judge Judy Sheindlin's You can't judge a book by its cover :|cool rules for school /          </td></tr>
	<tr><td>A young child's Bible                                                                       </td><td>A young child's Bible /                                                                       </td></tr>
	<tr><td>God's Kingdom: stories from the New Testament                                               </td><td>God's Kingdom :|stories from the New Testament /                                              </td></tr>
	<tr><td>Roman myths                                                                                 </td><td>Roman myths /                                                                                 </td></tr>
	<tr><td>Greek gods and goddesses                                                                    </td><td>Greek gods and goddesses /                                                                    </td></tr>
	<tr><td>The story of Shabbat                                                                        </td><td>The story of Shabbat /                                                                        </td></tr>
	<tr><td>On Shabbat                                                                                  </td><td>On Shabbat /                                                                                  </td></tr>
	<tr><td>On Purim                                                                                    </td><td>On Purim /                                                                                    </td></tr>
	<tr><td>Coming to America: the story of immigration                                                 </td><td>Coming to America :|the story of immigration /                                                </td></tr>
	<tr><td>The shared heart: portraits and stories celebrating lesbian, gay, and bisexual young people </td><td>The shared heart :|portraits and stories celebrating lesbian, gay, and bisexual young people /</td></tr>
	<tr><td>Remember the ladies: 100 great American women                                               </td><td>Remember the ladies :|100 great American women /                                              </td></tr>
	<tr><td>One Belfast boy                                                                             </td><td>One Belfast boy /                                                                             </td></tr>
	<tr><td>One boy from Kosovo                                                                         </td><td>One boy from Kosovo /                                                                         </td></tr>
	<tr><td>Slavery: bondage throughout history                                                         </td><td>Slavery :|bondage throughout history /                                                        </td></tr>
	<tr><td>River town                                                                                  </td><td>River town /                                                                                  </td></tr>
	<tr><td>Kids on strike!                                                                             </td><td>Kids on strike! /                                                                             </td></tr>
	<tr><td>A kids' guide to America's Bill of Rights: curfews, censorship, and the 100-pound giant     </td><td>A kids' guide to America's Bill of Rights :|curfews, censorship, and the 100-pound giant /    </td></tr>
	<tr><td>Do you remember the color blue?: and other questions kids ask about blindness               </td><td>Do you remember the color blue? :|and other questions kids ask about blindness /              </td></tr>
	<tr><td>The wildlife detectives: how forensic scientists fight crimes against nature                </td><td>The wildlife detectives :|how forensic scientists fight crimes against nature /               </td></tr>
	<tr><td>The environmental movement: from its roots to the challenges of a new century               </td><td>The environmental movement :|from its roots to the challenges of a new century /              </td></tr>
	<tr><td>After the spill: the Exxon Valdez disaster, then and now                                    </td><td>After the spill :|the Exxon Valdez disaster, then and now /                                   </td></tr>
	<tr><td>Juvenile crime                                                                              </td><td>Juvenile crime /                                                                              </td></tr>
	<tr><td>One-room school                                                                             </td><td>One-room school /                                                                             </td></tr>
	<tr><td>Reader's choice: the books children love to read and teachers love to teach.                </td><td>Reader's choice :|the books children love to read and teachers love to teach.                 </td></tr>
	<tr><td>Reader's choice: the books children love to read and teachers love to teach.                </td><td>Reader's choice :|the books children love to read and teachers love to teach.                 </td></tr>
	<tr><td>⋮</td><td>⋮</td></tr>
	<tr><td>Lectura Scott Foresman                                                                        </td><td>Lectura Scott Foresman /                                                                        </td></tr>
	<tr><td>Employment dispute resolution and worker rights in the changing workplace                     </td><td>Employment dispute resolution and worker rights in the changing workplace /                     </td></tr>
	<tr><td>The best teacher "stuff"                                                                      </td><td>The best teacher "stuff" /                                                                      </td></tr>
	<tr><td>Rosso in Italy: the Italian career of Rosso Fiorentino                                        </td><td>Rosso in Italy :|the Italian career of Rosso Fiorentino /                                       </td></tr>
	<tr><td>CL locker keys                                                                                </td><td>CL locker keys                                                                                  </td></tr>
	<tr><td>Many peoples, one land: a guide to new multicultural literature for children and young adults </td><td>Many peoples, one land :|a guide to new multicultural literature for children and young adults /</td></tr>
	<tr><td>Conceptual revolutions                                                                        </td><td>Conceptual revolutions /                                                                        </td></tr>
	<tr><td>The journals of gerontology.                                                                  </td><td>The journals of gerontology.                                                                    </td></tr>
	<tr><td>U.S. master tax guide.                                                                        </td><td>U.S. master tax guide.                                                                          </td></tr>
	<tr><td>Central business review.                                                                      </td><td>Central business review.                                                                        </td></tr>
	<tr><td>Steamboat!: the story of Captain Blanche Leathers                                             </td><td>Steamboat! :|the story of Captain Blanche Leathers /                                            </td></tr>
	<tr><td>Collections.|a Harcourt reading/language arts program.                                        </td><td>Collections.|a Harcourt reading/language arts program.                                          </td></tr>
	<tr><td>Co-operation in international business: the use of limited equity arrangements                </td><td>Co-operation in international business :|the use of limited equity arrangements /               </td></tr>
	<tr><td>McGraw-Hill reading.                                                                          </td><td>McGraw-Hill reading.                                                                            </td></tr>
	<tr><td>McGraw-Hill lectura                                                                           </td><td>McGraw-Hill lectura /                                                                           </td></tr>
	<tr><td>Collections: a Harcourt reading program                                                       </td><td>Collections :|a Harcourt reading program /                                                      </td></tr>
	<tr><td>Voz del lector: los libros que los ninos quieren leer Y los maestros quieren ensenar.         </td><td>Voz del lector :|los libros que los ninos quieren leer Y los maestros quieren ensenar.          </td></tr>
	<tr><td>Early supplementary feeding and cognition: effects over two decades                           </td><td>Early supplementary feeding and cognition :|effects over two decades /                          </td></tr>
	<tr><td>Collections: a Harcourt reading/language arts program                                         </td><td>Collections :|a Harcourt reading/language arts program /                                        </td></tr>
	<tr><td>Lectura Scott Foresman                                                                        </td><td>Lectura Scott Foresman /                                                                        </td></tr>
	<tr><td>McGraw-Hill lectura                                                                           </td><td>McGraw-Hill lectura /                                                                           </td></tr>
	<tr><td>McGraw-Hill lectura                                                                           </td><td>McGraw-Hill lectura /                                                                           </td></tr>
	<tr><td>Vamos de fiesta!: a Harcourt Spanish reading/language arts program                            </td><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                           </td></tr>
	<tr><td>Scott Foresman reading                                                                        </td><td>Scott Foresman reading /                                                                        </td></tr>
	<tr><td>Vamos de fiesta!: a Harcourt Spanish reading/language arts program                            </td><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                           </td></tr>
	<tr><td>Population today.                                                                             </td><td>Population today.                                                                               </td></tr>
	<tr><td>Practical plays                                                                               </td><td>Practical plays /                                                                               </td></tr>
	<tr><td><span style=white-space:pre-wrap>Science, technology &amp; human values.                                                           </span></td><td><span style=white-space:pre-wrap>Science, technology &amp; human values.                                                             </span></td></tr>
	<tr><td>CL locker keys                                                                                </td><td>CL locker keys                                                                                  </td></tr>
	<tr><td>Vamos de fiesta!: a Harcourt Spanish reading/language arts program                            </td><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                           </td></tr>
</tbody>
</table>




### Putting it all together with %>%

The [Pipe Operator](https://www.datacamp.com/community/tutorials/pipe-r-tutorial) `%>%` is loaded with the `tidyverse`. It takes the output of one statement and makes it the input of the next statement. You can think of it as “then” in natural language. So instead of making a bunch of intermediate data frames and cluttering up your workspace, you can run multiple functions at once. You can type the pipe with Ctrl + Shift + M if you have a PC or Cmd + Shift + M if you have a Mac.

So in the following example, the `books` tibble is first called, then the `format` is filtered to include only `book`, then only the `title` and `tot_chkout` columns are selected, and finally the data is rearranged from most to least checkouts.


```R
myBooks <- books %>%
  filter(format == "book") %>%
  select(title, tot_chkout) %>%
  arrange(desc(tot_chkout))
myBooks
```




<table class="dataframe">
<caption>A tibble: 6983 × 2</caption>
<thead>
	<tr><th scope=col>title</th><th scope=col>tot_chkout</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Click, clack, moo :|cows that type /                             </td><td>113</td></tr>
	<tr><td>The three pigs /                                                 </td><td>106</td></tr>
	<tr><td>Cook-a-doodle-doo! /                                             </td><td>103</td></tr>
	<tr><td>Because of Winn-Dixie /                                          </td><td> 79</td></tr>
	<tr><td>Uptown /                                                         </td><td> 69</td></tr>
	<tr><td>Through my eyes /                                                </td><td> 63</td></tr>
	<tr><td>Bud, not Buddy /                                                 </td><td> 63</td></tr>
	<tr><td>Brown bear, brown bear, what do you see? /                       </td><td> 61</td></tr>
	<tr><td>A year down yonder /                                             </td><td> 47</td></tr>
	<tr><td>Wemberly worried /                                               </td><td> 43</td></tr>
	<tr><td>Oso pardo, oso pardo, qué ves ahí? /                             </td><td> 41</td></tr>
	<tr><td>Discrete mathematics with applications /                         </td><td> 40</td></tr>
	<tr><td>Oh say can you seed? /                                           </td><td> 39</td></tr>
	<tr><td>Art since 1940 :|strategies of being /                           </td><td> 39</td></tr>
	<tr><td>Family /                                                         </td><td> 39</td></tr>
	<tr><td>Calculus and analytic geometry /                                 </td><td> 39</td></tr>
	<tr><td>The keeping quilt /                                              </td><td> 38</td></tr>
	<tr><td>Miss Spider's tea party /                                        </td><td> 38</td></tr>
	<tr><td>There's no place like space /                                    </td><td> 36</td></tr>
	<tr><td>Mufaro's beautiful daughters :|an African tale /                 </td><td> 36</td></tr>
	<tr><td>100th day worries /                                              </td><td> 35</td></tr>
	<tr><td>The yellow star :|the legend of King Christian X of Denmark /    </td><td> 35</td></tr>
	<tr><td>The tipping point :|how little things can make a big difference /</td><td> 35</td></tr>
	<tr><td>Cindy Ellen :|a wild western Cinderella /                        </td><td> 34</td></tr>
	<tr><td>Apples /                                                         </td><td> 34</td></tr>
	<tr><td>The hero with a thousand faces /                                 </td><td> 34</td></tr>
	<tr><td>Where do balloons go? :|an uplifting mystery /                   </td><td> 34</td></tr>
	<tr><td>Scott Foresman reading /                                         </td><td> 34</td></tr>
	<tr><td>Math advantage /                                                 </td><td> 34</td></tr>
	<tr><td>Roughing it on the Oregon Trail /                                </td><td> 33</td></tr>
	<tr><td>⋮</td><td>⋮</td></tr>
	<tr><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                                                                </td><td>0</td></tr>
	<tr><td>Fearon's world history /                                                                                                             </td><td>0</td></tr>
	<tr><td>National survey of problems and competencies among four- to sixteen-year-olds :|parents' reports for normative and clinical samples /</td><td>0</td></tr>
	<tr><td>Prentice Hall middle grades math :|tools for success /                                                                               </td><td>0</td></tr>
	<tr><td>Information literacy toolkit.                                                                                                        </td><td>0</td></tr>
	<tr><td>Index to artistic biography.                                                                                                         </td><td>0</td></tr>
	<tr><td>AWRA symposium on water resources and the World Wide Web, December 5-9, 1999, Seattle, Washington /                                  </td><td>0</td></tr>
	<tr><td>May Sarton :|a bibliography /                                                                                                        </td><td>0</td></tr>
	<tr><td>McGraw-Hill lectura /                                                                                                                </td><td>0</td></tr>
	<tr><td>Glencoe matemáticas :|aplicaciones y conexiones /                                                                                    </td><td>0</td></tr>
	<tr><td>Index to artistic biography.                                                                                                         </td><td>0</td></tr>
	<tr><td>Library of Congress Africana collections :|an illustrated guide.                                                                     </td><td>0</td></tr>
	<tr><td>McGraw-Hill lectura /                                                                                                                </td><td>0</td></tr>
	<tr><td>SRA Open Court reading. /                                                                                                            </td><td>0</td></tr>
	<tr><td>McGraw-Hill reading.                                                                                                                 </td><td>0</td></tr>
	<tr><td>SRA Open Court reading. /                                                                                                            </td><td>0</td></tr>
	<tr><td>McGraw-Hill reading.                                                                                                                 </td><td>0</td></tr>
	<tr><td>Guided participation in cultural activity by toddlers and caregivers /                                                               </td><td>0</td></tr>
	<tr><td>McGraw-Hill lectura /                                                                                                                </td><td>0</td></tr>
	<tr><td>Accounting handbook /                                                                                                                </td><td>0</td></tr>
	<tr><td>Collections :|a Harcourt reading program /                                                                                           </td><td>0</td></tr>
	<tr><td>Macmillan/McGraw-Hill reading/language arts :|a new view.                                                                            </td><td>0</td></tr>
	<tr><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                                                                </td><td>0</td></tr>
	<tr><td>Rosso in Italy :|the Italian career of Rosso Fiorentino /                                                                            </td><td>0</td></tr>
	<tr><td>Many peoples, one land :|a guide to new multicultural literature for children and young adults /                                     </td><td>0</td></tr>
	<tr><td>Conceptual revolutions /                                                                                                             </td><td>0</td></tr>
	<tr><td>Co-operation in international business :|the use of limited equity arrangements /                                                    </td><td>0</td></tr>
	<tr><td>Collections :|a Harcourt reading program /                                                                                           </td><td>0</td></tr>
	<tr><td>Early supplementary feeding and cognition :|effects over two decades /                                                               </td><td>0</td></tr>
	<tr><td>Vamos de fiesta! :|a Harcourt Spanish reading/language arts program /                                                                </td><td>0</td></tr>
</tbody>
</table>




#### Exercise: Playing with pipes `%>%`

1. Create a new data frame booksKids with these conditions:
    - filter() to include subCollection juvenile & k-12 materials and format books.
        - select() only title, call number, total checkouts, and publication year
        - arrange() by total checkouts in descending order
2. Use mean() to check the average number of checkouts for the booksJuv data frame.

### Split-apply-combine data analysis and the `summarize()` function

Many data analysis tasks can be approached using the split-apply-combine paradigm: split the data into groups, apply some analysis to each group, and then combine the results. `dplyr` makes this very easy through the use of the `group_by()` function.

#### The summarize() function

`group_by()` is often used together with `summarize()`, which collapses each group into a single-row summary of that group. `group_by()` takes as arguments the column names that contain the categorical variables for which you want to calculate the summary statistics.

So to compute the average checkouts by format:


```R
books %>%
  group_by(format) %>%
  summarize(mean_checkouts = mean(tot_chkout))
```




<table class="dataframe">
<caption>A tibble: 10 × 2</caption>
<thead>
	<tr><th scope=col>format</th><th scope=col>mean_checkouts</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>book        </td><td> 3.227123013</td></tr>
	<tr><td>cd-rom      </td><td> 0.333333333</td></tr>
	<tr><td>database    </td><td> 0.000000000</td></tr>
	<tr><td>e-gov doc   </td><td> 0.040241449</td></tr>
	<tr><td>image       </td><td> 0.027522936</td></tr>
	<tr><td>kit/object  </td><td> 1.333333333</td></tr>
	<tr><td>map         </td><td>10.619047619</td></tr>
	<tr><td>microform   </td><td> 0.001216545</td></tr>
	<tr><td>online video</td><td> 0.000000000</td></tr>
	<tr><td>serial      </td><td> 0.000000000</td></tr>
</tbody>
</table>




Books and maps have the highest, and as we would expect, databases, online videos, and serials have zero checkouts.

Here is a more complex example:


```R
books %>% 
  filter(format == "book") %>%
  mutate(call_class = str_sub(callnumber, 1, 1)) %>%
  group_by(call_class) %>%
  summarize(count = n(),
            sum_tot_chkout = sum(tot_chkout)) %>%
  arrange(desc(sum_tot_chkout))
```




<table class="dataframe">
<caption>A tibble: 34 × 3</caption>
<thead>
	<tr><th scope=col>call_class</th><th scope=col>count</th><th scope=col>sum_tot_chkout</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>E </td><td> 487</td><td>3114</td></tr>
	<tr><td>NA</td><td> 459</td><td>3024</td></tr>
	<tr><td>H </td><td>1142</td><td>2902</td></tr>
	<tr><td>P </td><td> 800</td><td>2645</td></tr>
	<tr><td>F </td><td> 240</td><td>1306</td></tr>
	<tr><td>Q </td><td> 333</td><td>1305</td></tr>
	<tr><td>B </td><td> 426</td><td>1233</td></tr>
	<tr><td>R </td><td> 193</td><td> 981</td></tr>
	<tr><td>L </td><td> 358</td><td> 862</td></tr>
	<tr><td>5 </td><td>  60</td><td> 838</td></tr>
	<tr><td>N </td><td> 289</td><td> 703</td></tr>
	<tr><td>D </td><td> 353</td><td> 690</td></tr>
	<tr><td>T </td><td> 165</td><td> 451</td></tr>
	<tr><td>3 </td><td>  56</td><td> 416</td></tr>
	<tr><td>Z </td><td> 189</td><td> 287</td></tr>
	<tr><td>G </td><td> 216</td><td> 265</td></tr>
	<tr><td>9 </td><td>  39</td><td> 247</td></tr>
	<tr><td>8 </td><td>  21</td><td> 242</td></tr>
	<tr><td>6 </td><td>  22</td><td> 225</td></tr>
	<tr><td>J </td><td> 228</td><td> 173</td></tr>
	<tr><td>K </td><td>  54</td><td> 113</td></tr>
	<tr><td>7 </td><td>  14</td><td>  92</td></tr>
	<tr><td>C </td><td>  98</td><td>  92</td></tr>
	<tr><td>M </td><td>  34</td><td>  89</td></tr>
	<tr><td>S </td><td> 251</td><td>  46</td></tr>
	<tr><td>U </td><td>  19</td><td>  41</td></tr>
	<tr><td>2 </td><td>   7</td><td>  39</td></tr>
	<tr><td>I </td><td> 272</td><td>  31</td></tr>
	<tr><td>4 </td><td>   3</td><td>  24</td></tr>
	<tr><td>1 </td><td>   3</td><td>  19</td></tr>
	<tr><td>A </td><td>  61</td><td>  18</td></tr>
	<tr><td>0 </td><td>   3</td><td>  11</td></tr>
	<tr><td>V </td><td>   5</td><td>   6</td></tr>
	<tr><td>Y </td><td>  83</td><td>   5</td></tr>
</tbody>
</table>




Let’s break this down step by step:

- First we call the `books` data frame
- We then pipe through `filter()` to include only books
- We then create a new column with `mutate()` called `call_class` by using the `str_sub()` function to keep the first character of the `call_number` variable
- We then `group_by()` our newly created `call_class` variable
- We then create two summary columns by using `summarize()`
    - take the number `n()` of items per `call_class` and assign it to a column called `count`
    - take the the `sum()` of `tot_chkout` per `call_class` and assign the result to a column called `sum_tot_chkout`
- Finally, we arrange `sum_tot_chkout` in descending order, so we can see the class with the most total checkouts. We can see it is the `E` class (History of America), followed by `NA` (items with no call number data), followed by `H` (Social Sciences) and `P` (Language and Literature).

### Exporting data

Now that you have learned how to use `dplyr` to extract information from or summarize your raw data, you may want to export these new data sets to share them with your collaborators or for archival.

Similar to the `read_csv()` function used for reading CSV files into R, there is a `write_csv()` function that generates CSV files from data frames.

Before using `write_csv()`, we are going to create a new folder, `data_output`, in our working directory that will store this generated dataset. We don’t want to write generated datasets in the same directory as our raw data. It’s good practice to keep them separate. The `data` folder should only contain the raw, unaltered data, and should be left alone to make sure we don’t delete or modify it. In contrast, our script will generate the contents of the `data_output` directory, so even if the files it contains are deleted, we can always re-generate them.

In preparation for our next lesson on plotting, we are going to create a version of the dataset with most of the changes we made above. We will first read in the original, then make all the changes with pipes.




```R
books_reformatted <- read_csv("./data/books.csv") %>%
  rename(title = X245.ab,
         author = X245.c,
         callnumber = CALL...BIBLIO.,
         isbn = ISN,
         pubyear = X008.Date.One,
         subCollection = BCODE1,
         format = BCODE2,
         location = LOCATION,
         tot_chkout = TOT.CHKOUT,
         loutdate = LOUTDATE,
         subject = SUBJECT,
         callnumber2 = CALL...ITEM.) %>%
  mutate(pubyear = as.integer(pubyear),
         call_class = str_sub(callnumber, 1, 1),
         subCollection = recode(subCollection,
                                "-" = "general collection",
                                u = "government documents",
                                r = "reference",
                                b = "k-12 materials",
                                j = "juvenile",
                                s = "special collections",
                                c = "computer files",
                                t = "theses",
                                a = "archives",
                                z = "reserves"),
         format = recode(format,
                         a = "book",
                         e = "serial",
                         w = "microform",
                         s = "e-gov doc",
                         o = "map",
                         n = "database",
                         k = "cd-rom",
                         m = "image",
                         "5" = "kit/object",
                         "4" = "online video"))
```

    
    [36m──[39m [1m[1mColumn specification[1m[22m [36m────────────────────────────────────────────────────────[39m
    cols(
      CALL...BIBLIO. = [31mcol_character()[39m,
      X245.ab = [31mcol_character()[39m,
      X245.c = [31mcol_character()[39m,
      LOCATION = [31mcol_character()[39m,
      TOT.CHKOUT = [32mcol_double()[39m,
      LOUTDATE = [31mcol_character()[39m,
      SUBJECT = [31mcol_character()[39m,
      ISN = [31mcol_character()[39m,
      CALL...ITEM. = [31mcol_character()[39m,
      X008.Date.One = [31mcol_character()[39m,
      BCODE2 = [31mcol_character()[39m,
      BCODE1 = [31mcol_character()[39m
    )
    
    
    Warning message in mask$eval_all_mutate(quo):
    “NAs introduced by coercion”


This chunk of code read the CSV, renamed the variables, used `mutate()` in combination with `recode()` to recode the `format` and `subCollection` values, used `mutate()` in combination with `as.integer()` to coerce `pubyear` to integer, and used `mutate()` in combination with `str_sub` to create the new varable `call_class`.

We now write it to a CSV and put it in the `data/output` sub-directory:


```R
write_csv(books_reformatted, "./data_output/books_reformatted.csv")
```

## Help with dplyr
- Read more about dplyr at https://dplyr.tidyverse.org/.
- In your console, after loading library(dplyr), run vignette("dplyr") to read an extremely helpful explanation of how to use it.
- See the http://r4ds.had.co.nz/transform.html in Garrett Grolemund and Hadley Wickham’s book R for Data Science.
- Watch this Data School video: https://www.youtube.com/watch?v=jWjqLW-u3hc

### Key Points

- Use the dplyr package to manipulate dataframes.
- Use select() to choose variables from a dataframe.
- Use filter() to choose data based on values.
Use group_by() and summarize() to work with subsets of data.
Use mutate() to create new variables.
