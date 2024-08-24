---
layout: single
title: Profile
permalink: /profile/

---

<span style="font-size: 16px">
  <a href="/assets/pdf/Resume_PT.pdf">PDF Version</a> | <a href="/assets/pdf/Resume_PT.pdf" download>Download</a>
</span>

## <u>Education</u>

**COLUMBIA UNIVERSITY**, New York City

M.S. in **Data Science**, Fu Foundation School of Engineering and Applied Science 		

**TONGJI UNIVERSITY**, Shanghai

BS in **Bioinformatics**, Minor in **Software Engineering** 															     	

## <u>Skills</u>

- ***Programming***: **Python**, **R**, **SQL**, **Java**, C++, C#, shell, HTML/CSS/JavaScript
- ***Data Science***: **Numpy**, **Pandas**, **Scipy**, **sklearn**, **PyTorch**, TensorFlow, **tidyverse**, ggplot2, **Shiny**, Power BI
- ***Software Development***: **SpringBoot**, **MySQL**, SQLServer, MongoDB, **Docker**, React.js, Axios.js, Node.js, Bootstrap, Flask, JUnit
- ***Concepts***: **Machine Learning**, **Deep Learning**, **Natural Language Processing**, **Computer Vision**, **Object-Oriented Programming**, **Data Structure**, RESTful API, **RDBMS**, NoSQL, **Agile Development**, Cloud Computing (**AWS**, Google Cloud, Azure, Alibaba Cloud)

## <u>Research</u>
**Transformer-based Peptide Language Model**

*AIQuraishi Laboratory, Columbia University*

- Collected and tidied 600k+ peptide datasets and 35 protein property datasets, ensuring high-quality data for model training and benchmark

- Trained transformer-based language models with PyTorch Lightning on Slurm-supported HPC to generate peptide/protein representation

- Conducted benchmark with various methods like Neural Network, Query Attention and Contrastive Learning on tasks such as motif detection


**Gradient Boosting Localization Algorithm for Disease Causal Variant Identification** 				

*Columbia University Irving Medical Center*										  					   

- Packed source code into an open-source **R** library while delivering correctness verification and unit testing for the algorithm blocks.

- Refactored source code with **R6 Class** to enhance the succinctness and reproducibility of source code.
- Accelerated the running time of algorithm by optimizing linear algebra calculation process and implementing core module with **C++** library like Eigen.

**Evaluation of Reference Datasets Oriented to Single-cell Annotations**								

*Tongji University*  																					  

- Designed the sampling method based on the determinantal point process and oversampling algorithm, which can generate high-quality reference data while keeping the original data amount.
- Implemented the method by **Python** and packaged code into a library with an MIT open-source license.
- Evaluated the method by comparing the automatic annotation results between original and refined datasets via several models, including Seurat, SciBet, scmap, etc.

**PhageMAP (Phage-Microbiome Assist Phagotherapy)** 									  		

*Best Software Award in iGEM (International Genetic Engineering Machine) 2021*  								

- Developed phagotherapy assisting method for analyzing bacterium-phage interactions by using algorithms including CRISPRdetect, BLAST, PHP, WIsh and statistical methods including the kernel.
- Designed and implemented a relational database that stored information of bacterium, phage and bacterium-phage interactions via **MySQL** platform.
- Offered a user-friendly usage of analysis procedure by deploying the software environment and packages in a **Docker** container and uploading the container to DockerHub with instructional documentation.
- Implemented the front-end [webpages](https://2021.igem.org/Team:Tongji_Software) using **HTML/CSS/JavaScript** collaboratively.

## <u>Internship</u>
**Radical AI Inc**

*AI Engineer Intern*

- Engineered a chat-based learning assistant using the Google Gemini model, featuring automated quiz generation and customized instruction

- Developed a robust FastAPI backend to process diverse file formats (YouTube videos, Microsoft documents, etc.) with LangChain and ChromaDB

- Ensured high performance through meticulous unit testing with pytest and comprehensive integration testing within Docker environments

**Shanghai Foxhub Network Technology Company**											

*Data Engineer Intern*

- Designed relational database architecture (ER diagrams) and unstructured data source (OSS, **Object Storage Service**) on the **Alibaba Cloud** platform to support front-end webpage access.

- Managed **MySQL** database access permission and backup operation by writing **shell** scripts to maintain the stability of
  production and development environment.


## <u>Projects</u>

**Custom LLM Chatbots with Character-Specific Tone** | *LLM, LoRA, NLP*

- Automated the collection of chat datasets from public wiki websites using a web scraper built with BeautifulSoup and Selenium in Python

- Fine-tuned state-of-the-art LLM like LLaMA using LoRA technique on the HuggingFace platform with customized dataset to acquire specific tone

- Developed RESTful API with FastAPI as backend and a multi-page app with Streamlit as frontend for interactive usage of cutomized models

**Billionaire Omics** | *R, Shiny, Bootstrap*

- Conducted exhaustive exploratory data analysis (EDA) with **tidyverse** to uncover patterns and insights related to billionaires.
- Developed a Shiny App for interactive data exploration, featuring intuitive navigation and dynamic visualizations.
- Implemented a [website](https://sitianzhou.github.io/BillionaireOmics/index.html) with **Bootstrap** and deployed **GitHub Pages** to serve as a comprehensive platform for showcasing project findings, ensuring a responsive and engaging user experience across various devices.

**Course Management System** | *SpringBoot, React.js, Bootstrap, MySQL, MongoDB, Docker, Maven, Webpack, Postman*

- Led the development of microservice-based system using **SpringBoot** and **React.js** as framework including requirement specification, system design, implementation and testing, resulting in a multi-functional and user-friendly web service application.
- Designed the database structure applying **MySQL** for relation-based data and **MongoDB** for archive-based data, while using **Docker** to maintain the isolation of different database.
- Implemented and tested RESTful API by **SpringBoot** and **Postman** while delivering an interactive website with **React.js, Node.js, Axios.js, Bootstrap, Webpack**.

**Predicting Solid Waste Composition with Neural Network** | *PyTorch, TensorFlow, sklearn*

- Implemented data cleaning, preprocessing and feature engineering for raw datasets with packages like **pandas, sklearn**.
- Predicted solid waste composition using neural network model while utilizing many machine learning technologies including L2 regularization, Adam optimizer and dropout layer via **PyTorch** library.
- Visualized data features and model evaluation results via **matplotlib** and **Tensorflow** library.

**Neurodegenerative Diseases Onset Prediction** | *Python, Flask, sklearn, MySQL*

- Completed data collection, preprocessing and feature engineering on open-source patient data about the onset of Alzheimer’s disease and Parkinson’s disease.
- Trained the prediction models in **sklearn** environment by classical machine learning algorithms including SVM, decision tree, etc.
- Delivered a project website with **Flask** to demonstrate details and provide an interactive prediction interface.