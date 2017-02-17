Obviously, the power of Spark is in applying it to large datasets with many features (like image categorisation, etc.). Because that's not very handy for demonstration purposes, I am here showing the principles on a small dataset.

Also, this dataset was a bit troublesome. The predictions were biased by the fact that the distributions were very different between countries. But I wanted to keep the analysis anyways. Because most blogs I've seen show only the cool examples, where ML worked really well, so that I thought it good to also show a difficult example and show what problems can arise. Maybe the model could be improved with feature engineering but here, I'm only showing a simple feature transformation example.

The reason why Nigeria, Cameroon, etc. had such high accuracy when we keep the 0s, is that they had a majority of 0 votes, meaning that the likelihood of correctly classifying a sample as 0 is high! When we remove 0s however, the prediction accuracy drops sharply. The main problem is that the distribution of preferences and whether a dish is known (i.e. has a value &gt; 0) is very different between countries. Specifically, whenever a value is overrepresented, the likelihood of correctly classifying that value by chance increases and thus introduces bias to our models!

------------------------------------------------------------------------

-   sparklyr: R interface for Apache Spark: <http://spark.rstudio.com/>

You can orchestrate machine learning algorithms in a Spark cluster via the machine learning functions within sparklyr. These functions connect to a set of high-level APIs built on top of DataFrames that help you create and tune machine learning workflows.

-   Apache Spark™ is a fast and general engine for large-scale data processing.: <http://spark.apache.org/>

Apache Spark is an open-source cluster-computing framework. Originally developed at the University of California, Berkeley's AMPLab, the Spark codebase was later donated to the Apache Software Foundation, which has maintained it since. Spark provides an interface for programming entire clusters with implicit data parallelism and fault-tolerance.

Overview\[edit\] Apache Spark provides programmers with an application programming interface centered on a data structure called the resilient distributed dataset (RDD), a read-only multiset of data items distributed over a cluster of machines, that is maintained in a fault-tolerant way.\[2\] It was developed in response to limitations in the MapReduce cluster computing paradigm, which forces a particular linear dataflow structure on distributed programs: MapReduce programs read input data from disk, map a function across the data, reduce the results of the map, and store reduction results on disk. Spark's RDDs function as a working set for distributed programs that offers a (deliberately) restricted form of distributed shared memory.\[3\]

The availability of RDDs facilitates the implementation of both iterative algorithms, that visit their dataset multiple times in a loop, and interactive/exploratory data analysis, i.e., the repeated database-style querying of data. The latency of such applications (compared to Apache Hadoop, a popular MapReduce implementation) may be reduced by several orders of magnitude.\[2\]\[4\] Among the class of iterative algorithms are the training algorithms for machine learning systems, which formed the initial impetus for developing Apache Spark.\[5\]

Apache Spark requires a cluster manager and a distributed storage system. For cluster management, Spark supports standalone (native Spark cluster), Hadoop YARN, or Apache Mesos.\[6\] For distributed storage, Spark can interface with a wide variety, including Hadoop Distributed File System (HDFS),\[7\] MapR File System (MapR-FS),\[8\] Cassandra,\[9\] OpenStack Swift, Amazon S3, Kudu, or a custom solution can be implemented. Spark also supports a pseudo-distributed local mode, usually used only for development or testing purposes, where distributed storage is not required and the local file system can be used instead; in such a scenario, Spark is run on a single machine with one executor per CPU core.

Spark Core\[edit\] Spark Core is the foundation of the overall project. It provides distributed task dispatching, scheduling, and basic I/O functionalities, exposed through an application programming interface (for Java, Python, Scala, and R) centered on the RDD abstraction (the Java API is available for other JVM languages, but is also usable for some other non-JVM languages, such as Julia,\[10\] that can connect to the JVM). This interface mirrors a functional/higher-order model of programming: a "driver" program invokes parallel operations such as map, filter or reduce on an RDD by passing a function to Spark, which then schedules the function's execution in parallel on the cluster.\[2\] These operations, and additional ones such as joins, take RDDs as input and produce new RDDs. RDDs are immutable and their operations are lazy; fault-tolerance is achieved by keeping track of the "lineage" of each RDD (the sequence of operations that produced it) so that it can be reconstructed in the case of data loss. RDDs can contain any type of Python, Java, or Scala objects.

Aside from the RDD-oriented functional style of programming, Spark provides two restricted forms of shared variables: broadcast variables reference read-only data that needs to be available on all nodes, while accumulators can be used to program reductions in an imperative style.\[2\]

A typical example of RDD-centric functional programming is the following Scala program that computes the frequencies of all words occurring in a set of text files and prints the most common ones. Each map, flatMap (a variant of map) and reduceByKey takes an anonymous function that performs a simple operation on a single data item (or a pair of items), and applies its argument to transform an RDD into a new RDD.

MLlib Machine Learning Library\[edit\] Spark MLlib is a distributed machine learning framework on top of Spark Core that, due in large part to the distributed memory-based Spark architecture, is as much as nine times as fast as the disk-based implementation used by Apache Mahout (according to benchmarks done by the MLlib developers against the Alternating Least Squares (ALS) implementations, and before Mahout itself gained a Spark interface), and scales better than Vowpal Wabbit.\[15\] Many common machine learning and statistical algorithms have been implemented and are shipped with MLlib which simplifies large scale machine learning pipelines, including:

summary statistics, correlations, stratified sampling, hypothesis testing, random data generation\[16\] classification and regression: support vector machines, logistic regression, linear regression, decision trees, naive Bayes classification collaborative filtering techniques including alternating least squares (ALS) cluster analysis methods including k-means, and Latent Dirichlet Allocation (LDA) dimensionality reduction techniques such as singular value decomposition (SVD), and principal component analysis (PCA) feature extraction and transformation functions optimization algorithms such as stochastic gradient descent, limited-memory BFGS (L-BFGS)

Spark offers over 80 high-level operators that make it easy to build parallel apps. And you can use it interactively from the Scala, Python and R shells. Spark powers a stack of libraries including SQL and DataFrames, MLlib for machine learning, GraphX, and Spark Streaming. You can combine these libraries seamlessly in the same application. Spark runs on Hadoop, Mesos, standalone, or in the cloud. It can access diverse data sources including HDFS, Cassandra, HBase, and S3. Spark’s primary abstraction is a distributed collection of items called a Resilient Distributed Dataset (RDD). RDDs can be created from Hadoop InputFormats (such as HDFS files) or by transforming other RDDs. Let’s make a new RDD from the text of the README file in the Spark source directory.

Spark is a fast and general processing engine compatible with Hadoop data. It can run in Hadoop clusters through YARN or Spark's standalone mode, and it can process data in HDFS, HBase, Cassandra, Hive, and any Hadoop InputFormat. It is designed to perform both batch processing (similar to MapReduce) and new workloads like streaming, interactive queries, and machine learning.

Many organizations run Spark on clusters of thousands of nodes. The largest cluster we know has 8000 of them. In terms of data size, Spark has been shown to work well up to petabytes. It has been used to sort 100 TB of data 3X faster than Hadoop MapReduce on 1/10th of the machines, winning the 2014 Daytona GraySort Benchmark, as well as to sort 1 PB. Several production workloads use Spark to do ETL and data analysis on PBs of data.

You can use either the standalone deploy mode, which only needs Java to be installed on each node, or the Mesos and YARN cluster managers. If you'd like to run on Amazon EC2, Spark provides EC2 scripts to automatically launch a cluster.

Note that you can also run Spark locally (possibly on multiple cores) without any special setup by just passing local\[N\] as the master URL, where N is the number of parallel threads you want.

No, but if you run on a cluster, you will need some form of shared file system (for example, NFS mounted at the same path on each node). If you have this type of filesystem, you can just deploy Spark in standalone mode.

At a high level, every Spark application consists of a driver program that runs the user’s main function and executes various parallel operations on a cluster. The main abstraction Spark provides is a resilient distributed dataset (RDD), which is a collection of elements partitioned across the nodes of the cluster that can be operated on in parallel. RDDs are created by starting with a file in the Hadoop file system (or any other Hadoop-supported file system), or an existing Scala collection in the driver program, and transforming it. Users may also ask Spark to persist an RDD in memory, allowing it to be reused efficiently across parallel operations. Finally, RDDs automatically recover from node failures.

A second abstraction in Spark is shared variables that can be used in parallel operations. By default, when Spark runs a function in parallel as a set of tasks on different nodes, it ships a copy of each variable used in the function to each task. Sometimes, a variable needs to be shared across tasks, or between tasks and the driver program. Spark supports two types of shared variables: broadcast variables, which can be used to cache a value in memory on all nodes, and accumulators, which are variables that are only “added” to, such as counters and sums.

This guide shows each of these features in each of Spark’s supported languages. It is easiest to follow along with if you launch Spark’s interactive shell – either bin/spark-shell for the Scala shell or bin/pyspark for the Python one.

-   Spark Machine Learning Library (MLlib): <http://spark.rstudio.com/mllib.html>

sparklyr provides bindings to Spark’s distributed machine learning library. In particular, sparklyr allows you to access the machine learning routines provided by the spark.ml package. Together with sparklyr’s dplyr interface, you can easily create and tune machine learning workflows on Spark, orchestrated entirely within R.

sparklyr provides three families of functions that you can use with Spark machine learning:

Machine learning algorithms for analyzing data (ml\_*) Feature transformers for manipulating individual features (ft\_*) Functions for manipulating Spark DataFrames (sdf\_\*) An analytic workflow with sparklyr might be composed of the following stages. For an example see Example Workflow.

Perform SQL queries through the sparklyr dplyr interface, Use the sdf\_\* and ft\_\* family of functions to generate new columns, or partition your data set, Choose an appropriate machine learning algorithm from the ml\_\* family of functions to model your data, Inspect the quality of your model fit, and use it to make predictions with new data. Collect the results for visualization and further analysis in R

Transformers A model is often fit not on a dataset as-is, but instead on some transformation of that dataset. Spark provides feature transformers, facilitating many common transformations of data within a Spark DataFrame, and sparklyr exposes these within the ft\_\* family of functions. These routines generally take one or more input columns, and generate a new output column formed as a transformation of those columns.

------------------------------------------------------------------------

Non-standard evaluation

2016-06-23

Dplyr uses non-standard evaluation (NSE) in all the important single table verbs: filter(), mutate(), summarise(), arrange(), select() and group\_by(). NSE is important not only because it reduces typing; for database backends, it’s what makes it possible to translate R code into SQL. However, while NSE is great for interactive use it’s hard to program with. This vignette describes how you can opt out of NSE in dplyr, and instead (with a little quoting) rely only on standard evaluation (SE).

Behind the scenes, NSE is powered by the lazyeval package. The goal is to provide an approach to NSE that you can learn once and then apply in many places (dplyr is the first of my packages to use this approach, but over time I will implement it everywhere). You may want to read the lazyeval vignettes, if you’d like to learn more about the underlying details, or if you’d like to use this approach in your own packages.

Standard evaluation basics

Every function in dplyr that uses NSE also has a version that uses SE. The name of the SE version is always the NSE name with an \_ on the end. For example, the SE version of summarise() is summarise\_(); the SE version of arrange() is arrange\_(). These functions work very similarly to their NSE cousins, but their inputs must be “quoted”:

It’s best to use a formula because a formula captures both the expression to evaluate and the environment where the evaluation occurs. This is important if the expression is a mixture of variables in a data frame and objects in the local environment

If you also want output variables to vary, you need to pass a list of quoted objects to the .dots argument

What if you need to mingle constants and variables? Use the handy lazyeval::interp()

------------------------------------------------------------------------

If you don't have Spark installed locally, run:

``` r
library(sparklyr)
spark_install(version = "2.0.0")
```

Now we can connect to a local Spark instance:

``` r
library(sparklyr)

sc <- spark_connect(master = "local", version = "2.0.0")
```

Before I start with my analysis, I am setting my custom ggplot2 theme and load the packages that I will definitely need: **tidyr** (gathering data for plotting), **dplyr** (data manipulation) and ggrepel (non-overlapping text labels in plots).

``` r
library(tidyr)
library(ggplot2)
library(ggrepel)
library(dplyr)

my_theme <- function(base_size = 12, base_family = "sans"){
  theme_minimal(base_size = base_size, base_family = base_family) +
  theme(
    axis.text = element_text(size = 12),
    axis.title = element_text(size = 14),
    panel.grid.major = element_line(color = "grey"),
    panel.grid.minor = element_blank(),
    panel.background = element_rect(fill = "aliceblue"),
    strip.background = element_rect(fill = "lightgrey", color = "grey", size = 1),
    strip.text = element_text(face = "bold", size = 12, color = "black"),
    legend.position = "right",
    legend.justification = "top", 
    panel.border = element_rect(color = "grey", fill = NA, size = 0.5)
  )
}
```

------------------------------------------------------------------------

The raw data behind the story "The FiveThirtyEight International Food Association's 2014 World Cup" <http://fivethirtyeight.com/features/the-fivethirtyeight-international-food-associations-2014-world-cup/>. For all the countries below, the response to the following question is presented: "Please rate how much you like the traditional cuisine of X"

5: I love this country's traditional cuisine. I think it's one of the best in the world.

4: I like this country's traditional cuisine. I think it's considerably above average.

3: I'm OK with this county's traditional cuisine. I think it's about average.

2: I dislike this country's traditional cuisine. I think it's considerably below average.

1: I hate this country's traditional cuisine. I think it's one of the worst in the world.

N/A: I'm unfamiliar with this country's traditional cuisine.

-   because I think that someone not knowing a country's cuisine is in itself information, I want NAs to be recoded as 0

``` r
library(fivethirtyeight)

food_world_cup[food_world_cup == "N/A"] <- NA
food_world_cup[, 9:48][is.na(food_world_cup[, 9:48])] <- 0
food_world_cup$gender <- as.factor(food_world_cup$gender)
food_world_cup$location <- as.factor(food_world_cup$location)
```

First I want to know what the distribution of preferences is for each country. Thus, I am calculating the percentages for each category and plot them with a pie chart that is facetted by country:

``` r
percentages <- food_world_cup %>%
  select(algeria:vietnam) %>%
  gather(x, y) %>%
  group_by(x, y) %>%
  summarise(n = n()) %>%
  mutate(Percent = round(n / sum(n) * 100, digits = 2))

# rename countries
percentages %>%
  mutate(x_2 = gsub("_", " ", x)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(x = "", y = Percent, fill = y)) + 
    geom_bar(width = 1, stat = "identity") + 
    theme_minimal() +
    coord_polar("y", start = 0) +
    facet_wrap(~ x_2, ncol = 8) +
    scale_fill_brewer(palette = "Set3") +
    labs(fill = "")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-7-1.png" style="display: block; margin: auto;" />

``` r
library(mice)

dataset_impute <- mice(food_world_cup[, -c(1, 2)],  print = FALSE)
food_world_cup <- cbind(food_world_cup[, 2, drop = FALSE], mice::complete(dataset_impute, 1))
food_world_cup[8:47] <- lapply(food_world_cup[8:47], as.numeric)
```

``` r
countries <- paste(colnames(food_world_cup)[-c(1:7)])

for (response in countries) {
  food_world_cup[paste(response, "trans", sep = "_")] <- food_world_cup[response] / mean(food_world_cup[food_world_cup[response] > 0, response])
}
```

``` r
food_world_cup %>%
  gather(x, y, algeria_trans:vietnam_trans) %>%
  mutate(x_2 = gsub("_trans", "", x)) %>%
  mutate(x_2 = gsub("_", " ", x_2)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(y)) +
    geom_density(fill = "navy", alpha = 0.7) +
    my_theme() + 
    facet_wrap(~ x_2, ncol = 8) +
    labs(x = "transformed preference")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-15-1.png" style="display: block; margin: auto;" />

### Do any countries show a gender difference?

``` r
food_world_cup_gather <- food_world_cup %>%
  collect %>%
  gather(country, value, algeria:vietnam)
                                 
food_world_cup_gather$value <- as.numeric(food_world_cup_gather$value)
food_world_cup_gather$country <- as.factor(food_world_cup_gather$country)
```

``` r
food_world_cup_gather <- food_world_cup_gather %>%
  mutate(x_2 = gsub("_", " ", country)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2))

order <- aggregate(food_world_cup_gather$value, by = list(food_world_cup_gather$x_2), FUN = sum)

food_world_cup_gather %>%
  mutate(x_2 = factor(x_2, levels = order$Group.1[order(order$x, decreasing = TRUE)])) %>%
  ggplot(aes(x = x_2, y = value, fill = gender)) +
    geom_bar(stat = "identity") +
    scale_fill_brewer(palette = "Set1") +
    my_theme() +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    labs(fill = "Gender",
         x = "",
         y = "sum of preferences")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-17-1.png" style="display: block; margin: auto;" />

``` r
food_world_cup %>%
  collect %>%
  mutate_each_(funs(as.numeric), countries) %>%
  group_by(gender) %>%
  summarise_each_(funs(mean), countries) %>%
  summarise_each_(funs(diff), countries) %>%
  gather(x, y) %>%
  mutate(x_2 = gsub("_", " ", x)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(x = x_2, y = y)) +
    geom_bar(stat = "identity", fill = "navy", alpha = 0.7) +
    my_theme() +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    labs(x = "",
         y = "difference\nbetween gender")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-19-1.png" style="display: block; margin: auto;" />

``` r
food_world_cup <- copy_to(sc, food_world_cup, overwrite = TRUE)
```

``` r
pca <- food_world_cup %>%
  mutate_each_(funs(as.numeric), countries) %>%
  ml_pca(features = paste(colnames(food_world_cup)[-c(1:47)]))

library(tibble)
as.data.frame(pca$components) %>%
  rownames_to_column(var = "labels") %>%
  mutate(x_2 = gsub("_trans", "", labels)) %>%
  mutate(x_2 = gsub("_", " ", x_2)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(x = PC1, y = PC2, color = x_2, label = x_2)) + 
    geom_point(size = 2, alpha = 0.6) +
    geom_text_repel() +
    labs(x = paste0("PC1: ", round(pca$explained.variance[1], digits = 2) * 100, "% variance"),
         y = paste0("PC2: ", round(pca$explained.variance[2], digits = 2) * 100, "% variance")) +
    my_theme() + 
    guides(fill = FALSE, color = FALSE)
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-22-1.png" style="display: block; margin: auto;" />

``` r
food_world_cup <- tbl(sc, "food_world_cup") %>%
  ft_string_indexer(input_col = "interest", output_col = "interest_idx") %>%
  ft_string_indexer(input_col = "gender", output_col = "gender_idx") %>%
  ft_string_indexer(input_col = "age", output_col = "age_idx") %>%
  ft_string_indexer(input_col = "household_income", output_col = "household_income_idx") %>%
  ft_string_indexer(input_col = "education", output_col = "education_idx") %>%
  ft_string_indexer(input_col = "location", output_col = "location_idx") %>%
  ft_string_indexer(input_col = "knowledge", output_col = "knowledge_idx")
```

``` r
partitions <- food_world_cup %>%
  sdf_partition(training = 0.75, test = 0.25, seed = 753)
```

``` r
library(lazyeval)

for(response in countries) {

  features <- colnames(partitions$training)[-grep(response, colnames(partitions$training))]
  features <- features[grep("_trans|_idx", features)]

  fit <- partitions$training %>%
    filter_(interp(~ var > 0, var = as.name(response))) %>%
    ml_random_forest(intercept = FALSE, response = response, features = features, type = "classification")
  
  feature_imp <- ml_tree_feature_importance(sc, fit)
  
  features <- as.character(feature_imp[1:10, 2])
  
  fit <- partitions$training %>%
    filter_(interp(~ var > 0, var = as.name(response))) %>%
    ml_random_forest(intercept = FALSE, response = response, features = features, type = "classification")
  
  partitions$test <- partitions$test %>%
    filter_(interp(~ var > 0, var = as.name(response)))
  
  pred <- sdf_predict(fit, partitions$test) %>%
    collect
  
  pred_2 <- as.data.frame(table(pred[[response]], pred$prediction))
  pred_2$response <- response
  
  pred_sc <- select(pred, -rawPrediction, -probability)
  pred_sc <- copy_to(sc, pred_sc, overwrite = TRUE)
  
  feature_imp$response <- response
  
  f1 <- ml_classification_eval(pred_sc, response, "prediction", metric = "f1")
  wP <- ml_classification_eval(pred_sc, response, "prediction", metric = "weightedPrecision")
  wR <- ml_classification_eval(pred_sc, response, "prediction", metric = "weightedRecall")
  
  ml_eval <- data.frame(response = response,
                        f1 = f1,
                        weightedPrecision = wP,
                        weightedRecall = wR)
  
  if (response == "algeria") {
    feature_imp_df <- feature_imp
    ml_eval_df <- ml_eval
    pred_df <- pred_2
  } else {
    feature_imp_df <- rbind(feature_imp_df, feature_imp)
    ml_eval_df <- rbind(ml_eval_df, ml_eval)
    pred_df <- rbind(pred_df, pred_2)
  }
}
```

-   <https://en.wikipedia.org/wiki/Precision_and_recall>

In pattern recognition and information retrieval with binary classification, precision (also called positive predictive value) is the fraction of retrieved instances that are relevant, while recall (also known as sensitivity) is the fraction of relevant instances that are retrieved. Both precision and recall are therefore based on an understanding and measure of relevance.

Suppose a computer program for recognizing dogs in scenes from a video identifies 8 dogs in a scene containing 12 dogs and some cats. Of the 8 dogs identified, 5 are actually dogs (true positives), while the rest are cats (false positive). The program's precision is 5/8 while its recall is 5/12. When a search engine returns 30 pages only 20 of which were relevant while failing to return 40 additional relevant pages, its precision is 20/30 = 2/3 while its recall is 20/60 = 1/3. So, in this case, precision is "how useful the search results are", and recall is "how complete the results are".

In statistics, if the null hypothesis is that all and only the relevant items are retrieved, absence of type I and type II errors corresponds respectively to maximum precision (no false positive) and maximum recall (no false negative). The above pattern recognition example contained 8 − 5 = 3 type I errors and 12 − 5 = 7 type II errors. Precision can be seen as a measure of exactness or quality, whereas recall is a measure of completeness or quantity.

In simple terms, high precision means that an algorithm returned substantially more relevant results than irrelevant ones, while high recall means that an algorithm returned most of the relevant results.

In an information retrieval scenario, the instances are documents and the task is to return a set of relevant documents given a search term; or equivalently, to assign each document to one of two categories, "relevant" and "not relevant". In this case, the "relevant" documents are simply those that belong to the "relevant" category. Recall is defined as the number of relevant documents retrieved by a search divided by the total number of existing relevant documents, while precision is defined as the number of relevant documents retrieved by a search divided by the total number of documents retrieved by that search.

In a classification task, the precision for a class is the number of true positives (i.e. the number of items correctly labeled as belonging to the positive class) divided by the total number of elements labeled as belonging to the positive class (i.e. the sum of true positives and false positives, which are items incorrectly labeled as belonging to the class). Recall in this context is defined as the number of true positives divided by the total number of elements that actually belong to the positive class (i.e. the sum of true positives and false negatives, which are items which were not labeled as belonging to the positive class but should have been).

In information retrieval, a perfect precision score of 1.0 means that every result retrieved by a search was relevant (but says nothing about whether all relevant documents were retrieved) whereas a perfect recall score of 1.0 means that all relevant documents were retrieved by the search (but says nothing about how many irrelevant documents were also retrieved).

In a classification task, a precision score of 1.0 for a class C means that every item labeled as belonging to class C does indeed belong to class C (but says nothing about the number of items from class C that were not labeled correctly) whereas a recall of 1.0 means that every item from class C was labeled as belonging to class C (but says nothing about how many other items were incorrectly also labeled as belonging to class C).

Often, there is an inverse relationship between precision and recall, where it is possible to increase one at the cost of reducing the other. Brain surgery provides an illustrative example of the tradeoff. Consider a brain surgeon tasked with removing a cancerous tumor from a patient’s brain. The surgeon needs to remove all of the tumor cells since any remaining cancer cells will regenerate the tumor. Conversely, the surgeon must not remove healthy brain cells since that would leave the patient with impaired brain function. The surgeon may be more liberal in the area of the brain she removes to ensure she has extracted all the cancer cells. This decision increases recall but reduces precision. On the other hand, the surgeon may be more conservative in the brain she removes to ensure she extracts only cancer cells. This decision increases precision but reduces recall. That is to say, greater recall increases the chances of removing healthy cells (negative outcome) and increases the chances of removing all cancer cells (positive outcome). Greater precision decreases the chances of removing healthy cells (positive outcome) but also decreases the chances of removing all cancer cells (negative outcome).

Usually, precision and recall scores are not discussed in isolation. Instead, either values for one measure are compared for a fixed level at the other measure (e.g. precision at a recall level of 0.75) or both are combined into a single measure. Examples for measures that are a combination of precision and recall are the F-measure (the weighted harmonic mean of precision and recall), or the Matthews correlation coefficient, which is a geometric mean of the chance-corrected variants: the regression coefficients Informedness (DeltaP') and Markedness (DeltaP).\[1\]\[2\] Accuracy is a weighted arithmetic mean of Precision and Inverse Precision (weighted by Bias) as well as a weighted arithmetic mean of Recall and Inverse Recall (weighted by Prevalence).\[1\] Inverse Precision and Recall are simply the Precision and Recall of the inverse problem where positive and negative labels are exchanged (for both real classes and prediction labels). Recall and Inverse Recall, or equivalently true positive rate and false positive rate, are frequently plotted against each other as ROC curves and provide a principled mechanism to explore operating point tradeoffs. Outside of Information Retrieval, the application of Recall, Precision and F-measure are argued to be flawed as they ignore the true negative cell of the contingency table, and they are easily manipulated by biasing the predictions.\[1\] The first problem is 'solved' by using Accuracy and the second problem is 'solved' by discounting the chance component and renormalizing to Cohen's kappa, but this no longer affords the opportunity to explore tradeoffs graphically. However, Informedness and Markedness are Kappa-like renormalizations of Recall and Precision,\[3\] and their geometric mean Matthews correlation coefficient thus acts like a debiased F-measure.

Precision takes all retrieved documents into account, but it can also be evaluated at a given cut-off rank, considering only the topmost results returned by the system. This measure is called precision at n or <P@n>.

For example for a text search on a set of documents precision is the number of correct results divided by the number of all returned results.

Precision is also used with recall, the percent of all relevant documents that is returned by the search. The two measures are sometimes used together in the F1 Score (or f-measure) to provide a single measurement for a system.

Note that the meaning and usage of "precision" in the field of information retrieval differs from the definition of accuracy and precision within other branches of science and technology.

For example for text search on a set of documents recall is the number of correct results divided by the number of results that should have been returned.

In binary classification, recall is called sensitivity. So it can be looked at as the probability that a relevant document is retrieved by the query.

It is trivial to achieve recall of 100% by returning all documents in response to any query. Therefore, recall alone is not enough but one needs to measure the number of non-relevant documents also, for example by computing the precision.

For classification tasks, the terms true positives, true negatives, false positives, and false negatives (see Type I and type II errors for definitions) compare the results of the classifier under test with trusted external judgments. The terms positive and negative refer to the classifier's prediction (sometimes known as the expectation), and the terms true and false refer to whether that prediction corresponds to the external judgment (sometimes known as the observation).

It is possible to interpret precision and recall not as ratios but as probabilities:

Precision is the probability that a (randomly selected) retrieved document is relevant. Recall is the probability that a (randomly selected) relevant document is retrieved in a search. Note that the random selection refers to a uniform distribution over the appropriate pool of documents; i.e. by randomly selected retrieved document, we mean selecting a document from the set of retrieved documents in a random fashion. The random selection should be such that all documents in the set are equally likely to be selected.

Note that, in a typical classification system, the probability that a retrieved document is relevant depends on the document. The above interpretation extends to that scenario also (needs explanation).

Another interpretation for precision and recall is as follows. Precision is the average probability of relevant retrieval. Recall is the average probability of complete retrieval. Here we average over multiple retrieval queries.

A measure that combines precision and recall is the harmonic mean of precision and recall, the traditional F-measure or balanced F-score:

This measure is approximately the average of the two when they are close, and is more generally the harmonic mean, which, for the case of two numbers, coincides with the square of the geometric mean divided by the arithmetic mean. There are several reasons that the F-score can be criticized in particular circumstances due to its bias as an evaluation metric.\[1\] This is also known as the {F\_{1}} F\_{1} measure, because recall and precision are evenly weighted.

Sensitivity and specificity are statistical measures of the performance of a binary classification test, also known in statistics as classification function:

Sensitivity (also called the true positive rate, the recall, or probability of detection\[1\] in some fields) measures the proportion of positives that are correctly identified as such (e.g., the percentage of sick people who are correctly identified as having the condition). Specificity (also called the true negative rate) measures the proportion of negatives that are correctly identified as such (e.g., the percentage of healthy people who are correctly identified as not having the condition). Another way to understand in the context of medical tests is that sensitivity is the extent to which true positives are not missed/overlooked (so false negatives are few) and specificity is the extent to which positives really represent the condition of interest and not some other condition being mistaken for it (so false positives are few). Thus a highly sensitive test rarely overlooks a positive (for example, showing "nothing bad" despite something bad existing); a highly specific test rarely registers a positive for anything that is not the target of testing (for example, finding one bacterial species when another closely related one is the true target); and a test that is highly sensitive and highly specific does both, so it "rarely overlooks a thing that it is looking for" and it "rarely mistakes anything else for that thing." Because most medical tests do not have sensitivity and specificity values above 99%, "rarely" does not equate to certainty. But for practical reasons, tests with sensitivity and specificity values above 90% have high credibility, albeit usually no certainty, in differential diagnosis.

Sensitivity therefore quantifies the avoiding of false negatives, and specificity does the same for false positives. For any test, there is usually a trade-off between the measures – for instance, in airport security since testing of passengers is for potential threats to safety, scanners may be set to trigger alarms on low-risk items like belt buckles and keys (low specificity), in order to increase the probability of identifying dangerous objects and minimize the risk of missing objects that do pose a threat (high sensitivity). This trade-off can be represented graphically using a receiver operating characteristic curve. A perfect predictor would be described as 100% sensitive (e.g., all sick individuals are correctly identified as sick) and 100% specific (e.g., no healthy individuals are incorrectly identified as sick); in reality any non-deterministic predictor will possess a minimum error bound known as the Bayes error rate.

-   F1: <https://en.wikipedia.org/wiki/F1_score>

In statistical analysis of binary classification, the F1 score (also F-score or F-measure) is a measure of a test's accuracy. It considers both the precision p and the recall r of the test to compute the score: p is the number of correct positive results divided by the number of all positive results, and r is the number of correct positive results divided by the number of positive results that should have been returned. The F1 score can be interpreted as a weighted average of the precision and recall, where an F1 score reaches its best value at 1 and worst at 0.

The traditional F-measure or balanced F-score (F1 score) is the harmonic mean of precision and recall — multiplying the constant of 2 scales the score to 1 when both recall and precision are 1.

This is related to the field of binary classification where recall is often termed as Sensitivity.

The F1 measure is the harmonic mean, or weighted average, of the precision and recall scores. The F1 measure penalizes classifiers with imbalanced precision and recall scores, like the trivial classifier that always predicts the positive class. A model with perfect precision and recall scores will achieve an F1 score of one. A model with a perfect precision score and a recall score of zero will achieve an F1 score of zero.

-   <https://en.wikipedia.org/wiki/Harmonic_mean#Harmonic_mean_of_two_numbers>

In mathematics, the harmonic mean (sometimes called the subcontrary mean) is one of several kinds of average, and in particular one of the Pythagorean means. Typically, it is appropriate for situations when the average of rates is desired. The harmonic mean can be expressed as the reciprocal of the arithmetic mean of the reciprocals.

``` r
results <- ml_eval_df %>%
  mutate(x_2 = gsub("_", " ", response)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2))
  
order <- results$x_2[order(results$weightedPrecision, decreasing = TRUE)]

gather(results, x, y, f1:weightedRecall) %>%
  mutate(x_2 = factor(x_2, levels = order)) %>%
  ggplot(aes(x = x_2, y = y, fill = x)) +
    geom_bar(stat = "identity", position = "dodge", alpha = 0.9) +
    scale_fill_brewer(palette = "Set1") +
    my_theme() +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    labs(fill = "", color = "", x = "", y = "value")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-40-1.png" style="display: block; margin: auto;" />

``` r
feats <- feature_imp_df %>%
  filter(response == "spain") %>%
  slice(1:10)

as.data.frame(food_world_cup) %>%
  select_(.dots = c("spain", as.character(feats$feature))) %>%
  gather(x, y, -spain) %>%
  filter(spain > 0) %>%
  mutate(x_2 = gsub("_trans", "", x)) %>%
  mutate(x_2 = gsub("_idx", "", x_2)) %>%
  mutate(x_2 = gsub("_", " ", x_2)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(x = x_2, y = spain, color = y)) +
    geom_jitter(alpha = 0.2) +
    scale_color_gradient(low = "blue", high = "red") +
    my_theme() +
    theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust = 1)) +
    labs(x = "", y = "Spain", color = "")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-42-1.png" style="display: block; margin: auto;" />

``` r
feats <- feature_imp_df %>%
  filter(response == "greece") %>%
  slice(1:10)

as.data.frame(food_world_cup) %>%
  select_(.dots = c("greece", as.character(feats$feature))) %>%
  gather(x, y, -greece) %>%
  filter(greece > 0) %>%
  mutate(x_2 = gsub("_trans", "", x)) %>%
  mutate(x_2 = gsub("_idx", "", x_2)) %>%
  mutate(x_2 = gsub("_", " ", x_2)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(x = x_2, y = greece, color = y)) +
    geom_jitter(alpha = 0.2) +
    scale_color_gradient(low = "blue", high = "red") +
    my_theme() +
    theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust = 1)) +
    labs(x = "", y = "Greece", color = "")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-43-1.png" style="display: block; margin: auto;" />

``` r
feats <- feature_imp_df %>%
  filter(response == "italy") %>%
  slice(1:10)

as.data.frame(food_world_cup) %>%
  select_(.dots = c("italy", as.character(feats$feature))) %>%
  gather(x, y, -italy) %>%
  filter(italy > 0) %>%
  mutate(x_2 = gsub("_trans", "", x)) %>%
  mutate(x_2 = gsub("_idx", "", x_2)) %>%
  mutate(x_2 = gsub("_", " ", x_2)) %>%
  mutate(x_2 = gsub("(^|[[:space:]])([[:alpha:]])", "\\1\\U\\2", x_2, perl = TRUE)) %>%
  mutate(x_2 = gsub("And", "and", x_2)) %>%
  ggplot(aes(x = x_2, y = italy, color = y)) +
    geom_jitter(alpha = 0.2) +
    scale_color_gradient(low = "blue", high = "red") +
    my_theme() +
    theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust = 1)) +
    labs(x = "", y = "Italy", color = "")
```

<img src="food_spark_files/figure-markdown_github/unnamed-chunk-44-1.png" style="display: block; margin: auto;" />

------------------------------------------------------------------------

Next week, I'll be looking into H2O integration with sparklyr using rsparkling and Sparkling Water.

------------------------------------------------------------------------

Other ML posts:

------------------------------------------------------------------------

<br>

    ## R version 3.3.2 (2016-10-31)
    ## Platform: x86_64-w64-mingw32/x64 (64-bit)
    ## Running under: Windows 7 x64 (build 7601) Service Pack 1
    ## 
    ## locale:
    ## [1] LC_COLLATE=English_United States.1252 
    ## [2] LC_CTYPE=English_United States.1252   
    ## [3] LC_MONETARY=English_United States.1252
    ## [4] LC_NUMERIC=C                          
    ## [5] LC_TIME=English_United States.1252    
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] tibble_1.2            fivethirtyeight_0.1.0 dplyr_0.5.0          
    ## [4] ggrepel_0.6.5         ggplot2_2.2.1         tidyr_0.6.1          
    ## [7] sparklyr_0.5.3-9002  
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_0.12.9        RColorBrewer_1.1-2 plyr_1.8.4        
    ##  [4] base64enc_0.1-3    tools_3.3.2        digest_0.6.12     
    ##  [7] jsonlite_1.2       evaluate_0.10      gtable_0.2.0      
    ## [10] shiny_1.0.0        DBI_0.5-1          rstudioapi_0.6    
    ## [13] yaml_2.1.14        parallel_3.3.2     withr_1.0.2       
    ## [16] httr_1.2.1         stringr_1.1.0      knitr_1.15.1      
    ## [19] rappdirs_0.3.1     rprojroot_1.2      grid_3.3.2        
    ## [22] R6_2.2.0           rmarkdown_1.3      magrittr_1.5      
    ## [25] backports_1.0.5    scales_0.4.1       htmltools_0.3.5   
    ## [28] assertthat_0.1     mime_0.5           xtable_1.8-2      
    ## [31] colorspace_1.3-2   httpuv_1.3.3       labeling_0.3      
    ## [34] config_0.2         stringi_1.1.2      lazyeval_0.2.0    
    ## [37] munsell_0.4.3