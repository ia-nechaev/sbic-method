# sbic-method
Standard-Based Impact Classification (SBIC) method of CSR report analysis in accordance with GRI framework

current version of SBIC-method is trained to identify the following 10 codes:
401-Employment,
403-Occupational Health and Safety,
404-Training and Education,
405-Diversity and Equal Opportunity,
407-Freedom of Association and Collective Bargaining,
413-Local Communities,
414-Supplier Social Assessment,
416-Customer Health and Safety,
417-Marketing and Labeling
999-Unrelated

Usage is pretty staritforward. Each text entry is considered as text from one page of a report.

```{r}
## loading libraries

library(recipes)
library(workflows)
library(textrecipes)

library(tidyverse)
library(magrittr)
library(tidymodels)
library(parsnip)
```

```{r}
## Loading workflow with model

wf_fit<-readRDS("sbic_model.Rds")
```

```{r}
## Selecting column with text form a df

text_to_predict <- text_unnested %>% select(text)
```

```{r}
## Running the model

text_predicted <- wf_fit %>% predict(text_to_predict)
```

```{r}
## Stating the impact type

text_unnested %<>% mutate(gcode=text_predicted$.pred_class)
```
