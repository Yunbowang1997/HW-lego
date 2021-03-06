# HW-lego
```{r}
library(tidyverse)
library(dsbox)
```

**Question 1** What are the three most common first names of purchasers?

```{r}
lego_sales %>% group_by(first_name) %>% count(first_name) %>% 
  arrange(desc(n)) %>% head(5)
```

Answer: The three most common first names of purhasers are Jackson, Jacob and Joseph.

**Question 2** What are the three most common themes of Lego sets purchased?

```{r}
lego_sales %>% group_by(theme) %>% count(theme) %>% arrange(desc(n)) %>% head(5)
```

Answer: The three most common themes of Lego sets purchased are "Star Wars", "Nexo Knights" and "Gear".

**Question 3** Among the most common theme of Lego sets purchased, what is the most common subtheme?
```{r}
lego_sales %>% filter(theme=="Star Wars") %>% 
  group_by(subtheme) %>% count(subtheme) %>%
  arrange(desc(n)) %>% head(3)
```

Answer: Among the most common theme of Lego sets purchased, the most common subtheme is "The Force Awakens".

**Question 4** Create a new variable called age_group and group the ages into the following categories: “18 and under”, “19 - 25”, “26 - 35”, “36 - 50”, “51 and over”.

```{r}
lego_agegroup <- mutate(lego_sales, age_group = 
                          case_when(
                            age <= 18 ~ "18 and under",
                            age <= 25 ~ "19 - 25",
                            age <= 35 ~ "26 - 35",
                            age <= 50 ~ "36 - 50",
                            TRUE ~ "51 and over"
                          ))
```

**Question 5** Which age group has purchased the highest number of Lego sets.

```{r}
lego_agegroup %>% group_by(age_group) %>% count(age_group, wt = quantity) %>% 
  arrange(desc(n))
```

Answer: The age group of 36 - 50 has purchased the highest number of Lego sets.

**Question 6** Which age group has spent the most money on Legos?

```{r}
lego_agegroup %>% group_by(age_group) %>% 
  count(age_group, wt = us_price * quantity) %>%
  arrange(desc(n))The age group of 36 - 50 has spent the most money on Legos.

**Question 7** Which Lego theme has made the most money for Lego?

```{r}
lego_sales %>% group_by(theme) %>% count(theme, wt = us_price * quantity) %>%
  arrange(desc(n)) %>% head(3)
```

The Lego theme "Star Wars" has made the most money for lego.

**Question 8** Which area code has spent the most money on Legos? In the US the area code is the first 3 digits of a phone number.

```{r}
lego_sales %>% mutate(area = str_sub(phone_number, 1, 3)) %>% 
  group_by(area) %>% count(area, wt = us_price * quantity) %>%
  arrange(desc(n)) %>% head(3)
```

Answer: The area code 956 has spent the most money on Legos.

**Question 9** Come up with a question you want to answer using these data, and write it down. Then, create a data visualization that answers the question, and explain how your visualization answers the question.

```{r}
lego_sales %>% group_by(us_price) %>% count(us_price) %>% arrange(desc(n))
```

Answer: A question I want to answer using these data is: Which price of Lego sets purchased is most popular?

```{r}
ggplot(lego_sales, aes(us_price)) + geom_bar()
```

Using bar plot we found that the prices higher than 80 are less popular. So we limit the price from 0 to 80.

```{r}
ggplot(lego_sales, aes(us_price)) + geom_bar() + coord_cartesian(xlim = c(0, 80))
```

Now we found the price about 10 is most popular.
