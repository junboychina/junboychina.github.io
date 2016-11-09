---
layout: post_layout
title: Using Bayes Network for Genetic Inheritance
time: 2016年08月05日 Friday
location: 纽约
pulished: true
excerpt_separator: "```"
---
In my previous post [SAMIAM Introduction](http://sfadfs>), we have succeeded in helping the bank modele a simple Credit-worthness Predictor! That's fabulous! Do you have the feeling that we have found a new world! I really appreciate my teacher, Prof.Chen, who taught me the course - Probability and Stochastic Process. He inspired my interest in Probability and I was the top 3 student in his class. This lays a strong foundation for PGM.

Today, we will continue our exploration in PGM. This time, we will help Genetic Counselors advise couples with a family history of a genetic disease. Specifically, we need to help couples decide whether to have a biological child or to adopt by assessing the probability that their unborn child will have the disease. 

<img,NPC!>

## OVERVIEW
- Problem Analysis
- Bayes Network 
- Alternate Bayes Network
- Traits Controlled by Multiple Genes
- Reference

## 1. Problem Analysis

The question is so familiar for us, because it seems that this is just a common biological exercise in high school! Half and half. This is different from the high school exercise because we didn't learn Probability Theory in high school. For a high school student, if "E" is the dominant allele for double eyelids and "e" is the recessive allele, then a person with genotype "EE" will have the physical trait, double eyelids, with one hundred percent probability. This is called **Mendelian genetics**. In Mendelian genetics, a dominant allele is always expressed, and a recessive allele is expressed only when the domiant allele is not present. A person with genotype "EE" or "ee" is called homozygous for the gene for double eyelids and "Ee" is called heterozygous for the gene for double eyelids.

In fact, inheritance of many physical traits is not Mendelian. That's what we need to solve now. For example, the traits of polydactyly which is the trait of having an additional finger on one or both hands. Assuming there are two alleles for this gene, "P" and "p", where "P" is dominant. But here "P" doesn't cause the polydactyly with one hundred probability but is just a **risk factor**. While "p" might not prevent someone from having polydactyly but just make the person less likely to have polydactyly. For instance, "PP" might cause polydactyly with probability 0.8, while "pp" will cause polydactyly with probability 0.05. (It is not necessary that all the probability sum to 1)

Because Gene passed down from generation to generation and the relationship between genotype and phenotype is represented by a risk factor. If we use a graph to represent the whole relationship, it is convenient to use a directed graph. So we can use **Bayes Network** to model the mechanism of genetic inheritance.

Genetic inheritance patterns are generally consistent from generation to generation, so **template model** is a natural way to model them. Then parameters and structure are reused within all the Bayes Networks and across different Bayes Networks.

## 2. Bayes Network

> An example of Cystic Fibrosis: Given the family tree, allele frequency in the population, and the risk factor for all the genotypes. Construct the Bayes Network and help the Genetic Counselors to give advice to the couples in this family.

#### 2-1. Construct the Template
First, we need to construct a template which only contains a son(James), his mother(Ira) and father(Robin). Find the factors in this template, then construct Bayes Network for the whole family tree with those factors in the template. Template is as follows:

<img>

We find there are mainly three kinds of factors in this template:

>P(person’s phenotype | person’s genotype)
>
>P(person’s genotype | genotype of person’s first parent, genotype of person’s second parent)
>
>P(person’s genotype) , its values are based on the allele frequencies in the population

#### 2-2. Construct a Complete Bayes Network

#### 2-3. Giving Advice