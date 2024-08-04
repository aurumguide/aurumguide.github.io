---
title: "How to use Markdown and its grammar"
excerpt: ""

categories:
  - GitBlog
tags:
  - [GitBlog]

## permalink: /MsSql/Mssql_Install///

toc: true
toc_sticky: true
 
date: 2024-08-07
last_modified_at: 2024-08-07
---

I would like to introduce the Markdown language, which is used on most websites as a simple alternative to HTML when writing articles.

## <mark>What are the pros and cons of Markdown?</mark>

- The grammar is simple and concise, so most developers can understand and use it in 10 minutes.
- Markdown is very easy to manage.
- Most programs and platforms support it.
- There is no Markdown standard, so you need to be familiar with the applicable grammar for each platform and program.
- Simple functions are supported, but tags must be used for detailed functions.

## <mark>How to use Markdown?</mark>

#### 1\. How to change title size in Markdown.

- In HTML grammar, they are used as h1, h2, h3, h4, h5, and h6.

```markdown
# aurumguide title1 
## aurumguide title2 
### aurumguide title3 
#### aurumguide title4  
##### aurumguide title5 
###### aurumguide title6
```

**Execution results :**

![How to change title size in Markdown.](/assets/images/postsImages/GitBlog/3003_Eng_md_file_grammer/1.png)

#### 2\. How to use Line Breaks.

```markdown
To break a line, type 3 spaces or use the   "br" tag. 

```

**Execution results :**   
> To break a line, type 3 spaces or use the   
> "br" tag.

#### 3\. How to use Division Line

- Division lines are indicated by '---','***'.
- You must use at least 3.

```markdown
Before Markdown Divider
----------------------------------------------------------------------
After Markdown Divider
```

**Execution results :**

Before Markdown Divider   
After Markdown Divider   


#### 4\. How to use indentation.

- Use indentation '>'.

```markdown
Markdown indentation not applied.
>>> Markdown indentation version 1.
>>>>> Markdown indentation version 2.
```

**Execution results :**

Markdown indentation not applied.
>>> Markdown indentation version 1.
>>>>> Markdown indentation version 2.

#### 5\. How to use Bullet.

- Mostly '-' is used, but '+', '\*' can be used.

```markdown
- Markdown bullet.
  - Markdown bullet.
  - Markdown bullet.
  - Markdown bullet.
```

**Execution results :**

> - Markdown bullet.
>   - Markdown bullet.
>   - Markdown bullet.
>   - Markdown bullet.

#### 6\. How to use numbers.

```markdown
1. Markdown Indexing1.
2. Markdown Indexing2.
3. Markdown Indexing3.
4. Markdown Indexing4.
5. Markdown Indexing5.
```

**Execution results :**

> 1. Markdown Indexing1.
> 2. Markdown Indexing2.
> 3. Markdown Indexing3.
> 4. Markdown Indexing4.
> 5. Markdown Indexing5.

#### 7\. Setting up a hyperlink.

- \<link address>
- \[link name\](link address)
- \[link name\](link address, "additional explanation")
- In the case of an additional explanation, it is visible when you hover the mouse over it.

```markdown
<https://aurumguide.com>
[Go to Aurum Guide] (https://aurumguide.com/)
[Go to Aurum Guide] (https://aurumguide.com/, "Go to the Auraum Guide.")
```

**Execution results :**

<https://aurumguide.com>   
[Go to Aurum Guide] (https://aurumguide.com/)   
[Go to Aurum Guide] (https://aurumguide.com/, "Go to the Auraum Guide.")

#### 8\. Set the image.

- ! \[Image Description\](Image Address)

```markdown
![Aurum Guide](/assets/images/postsImages/GitBlog/3001_Eng_Windws_Shortcuts/1.png)
```

**Execution results :**

![Aurum Guide.](/assets/images/postsImages/GitBlog/3001_Eng_Windws_Shortcuts/1.png)

#### 9\. How to use Markdown text emphasis.

**How to italicize text.**

- In Markdown, you can italicize text by using a single '\*'.
- You can also use the '<em>' tag.

```markdown
*Anyway, look at the guide's italicized text*
<em>Anyway, look at the guide's italicized text</em>
```

**Execution results :**

> *Anyway, look at the guide's italicized text*   
> <em>Anyway, look at the guide's italicized text</em>

**Highlight text.**

- In Markdown, you can highlight text by using two '\*\*'.
- You can also use the \<strong\> tag.

```markdown
\*\*See the highlighted text in the Amorum Guide**
<strong>See the highlighted text in the Amorum Guide</strong>
```

**Execution results :**

> **See the highlighted text in the Amorum Guide**   
> <strong>See the highlighted text in the Amorum Guide</strong>

**Italicized text, for emphasis.**

- In Markdown, you can use '\*\*\*' three times to italicize and emphasize text.

```markdown
*** Of course, highlight guide, italicize text view ***
```

**Execution results :**

> ***Of course, highlight guide, italicize text view.***   

**Underline text.**

- In Markdown, you can underline text using '\_\_'.
- You can also use the \<u> tag.

```markdown
<u>See the underlined text in the guide</u>
```

Execution results :    
> <u>See the underlined text in the guide</u>


**Strikethrough.**

- In Markdown, you can strikethrough using \<del>.

```markdown
<del> Show strikethrough text in any guide</del>
```

**Execution results :**   
  
> <del> Show strikethrough text in any guide</del>

**Applying Color to Text.**

- How to apply color to text in Markdown.
- Usually used with html tags.

```markdown
<span style="color:blue">
Of course, show guide colors
</span>
```

**Execution results :**

> <span style="color:blue">
> Of course, show guide colors
> </span>

#### 10\. How to use quotes in Markdown.

- You can use '>' as a quote in Markdown.
- If you want to use it in a hierarchical structure, you can use '>' continuously.

```markdown
> Aurum Guide Quote Mark 1.
>> Aurum Guide Quote Mark 2-1.
>> Aurum Guide Quote Mark 2-2.
>> Aurum Guide Quote Mark 3.
```

  **Execution results :**   

> Aurum Guide Quote Mark 1.
>> Aurum Guide Quote Mark 2-1.
>> Aurum Guide Quote Mark 2-2.
>> Aurum Guide Quote Mark 3.

#### 11\. How to use checkboxes in Markdown.

```markdown
[] An example of an unchecked checkbox.   
[x] An example of a checked checkbox.
```

**Execution results :**

> [] An example of an unchecked checkbox.   
> [x] An example of a checked checkbox.

#### 12\. This is the basic grammar for creating tables in Markdown.

- 'Table', which is converted to tags in HTML, is also possible in Markdown.
- To separate table headers, use three or more -(hyphen/dash) symbols.

```markdown
|Aurum1|Aurum2|Aurum3|Aurum4|Aurum5|
|---|---|---|---|---|
|Content1|Content2|Content3|Content4|Content5|
|Content1|Content2|Content3|Content4|Content5|
|Content1|Content2|Content3|Content4|Content5|
|Content1|Content2|Content3|Content4|Content5|
|Content1|Content2|Content3|Content4|Content5|
```

**Execution results :**

> |Aurum1|Aurum2|Aurum3|Aurum4|Aurum5|
> |---|---|---|---|---|
> |Content1|Content2|Content3|Content4|Content5|
> |Content1|Content2|Content3|Content4|Content5|
> |Content1|Content2|Content3|Content4|Content5|
> |Content1|Content2|Content3|Content4|Content5|
> |Content1|Content2|Content3|Content4|Content5|

**How ​​to align cells.**

- You can align content within a cell (column/field) by adding the :(Colons) symbol to separate table headers.
- :--- indicates left alignment.
- :---: indicates center alignment.
- \---: indicates right alignment.

```markdown
|Aurum1|Aurum2|Aurum3|Aurum4|Aurum5|
|:---|---|:---:|---|---:|
|LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
|LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
|LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
|LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
|LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
```

**Execution results :**

> |Aurum1|Aurum2|Aurum3|Aurum4|Aurum5|
> |:---|---|:---:|---|---:|
> |LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
> |LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
> |LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
> |LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|
> |LeftAlign1|Content2|CenterAlign3|Content4|RightAlign5|

**Tips for creating tables.**

- Using the vertical bar symbol
- In Markdown, the vertical bar (\|) symbol is a grammar used to express tables.
- If you want to output a vertical bar (\|) within a table, use the \\Escape symbol in front.

```markdown
When you want to put a space in the cell?
|Aurum1|Aurum2|Aurum3|
|:---|:---:|---:|
||Content1||
|||Content2|
|Content3|||
```

**Execution results :**

> |Aurum1|Aurum2|Aurum3|
> |:---|:---:|---:|
> ||Content1||
> |||Content2|
> |Content3|||

#### 13\. How to use code blocks in Markdown.

**How ​​to use code inline in Markdown.**

- You can display source code inline by entering it using a single backquote (\`) between sentences.

```markdown
The variable declaration code is `let numType = 123`.
```

**Execution results :**

> The variable declaration code is `let numType = 123`.

**How ​​to use it as a code block in Markdown.**

- You can use a code block by using three backquotes (\`).

```sql
 SELECT *
   FROM DUAL;
```

> SELECT *   
>   FROM DUAL;

#### 14\. How to use comments in Markdown.

- /\*\*/ represents a comment.
