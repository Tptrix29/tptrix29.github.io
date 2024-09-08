---
layout: single
title: Profile
permalink: /profile/
toc: false
classes: wide
---

<span style="font-size: 16px">
  <a href="/assets/pdf/Resume_PT.pdf">PDF Version</a> | <a href="/assets/pdf/Resume_PT.pdf" download>Download</a>
</span>

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: 'Times New Roman'
        }
        .section {
            margin-bottom: 20px;
        }
        .skills, .education, .experience, .projects {
            margin-bottom: 20px;
        }
        .section-title {
            font-weight: bold;
            font-size: 25px;
            margin-bottom: 5px;
            border-bottom: 1px solid #ccc;
            padding-bottom: 5px;
        }
        .subtitle{
            font-size: 22px;
            font-weight: bold;
        }
        .emph{
            font-size: 20px;
            font-weight: bold;
            font-style: italic;
        }
        .locale{
            font-size: 20px;
            font-style: italic;
            text-align: right;
            margin-left: auto
        }
        .timeline{
            font-size: 20px;
            text-align: right;
            margin-left: auto
        }
        .container{
            font-size: 22px;
            display: flex; 
            margin: 1px;
        }
        .bullets{
            margin: 0px;
            padding-top: 0px;
            padding-bottom: 0px;
            line-height: 1.5;
            font-size: 20px;
        }
        /* .proj-link{
            font-size: 16px;
        } */
    </style>
</head>
<body>
    <!-- Education -->
    <section class="education section">
        <div class="section-title">Education</div>
        <!-- MS -->
        <div class="container">
            <div class="subtitle">Columbia University, School of Engineering and Applied Science</div>
            <div class="locale">New York City, NY</div>
        </div>
        <div class="container">
            <div class="emph">M.S. in Data Science</div> 
            <div class="timeline">Sep 2023 – May 2025 (Expected)</div>
        </div>
        <div class="bullets">
        <strong>Core Courses</strong>: Machine Learning for Data Science, Natural Language Processing, Algorithms for Data Science, Applied Machine Learning, Data Science, Computer Systems for Data Science, Probability, Statistical Inference
        </div>
        <hr>
        <!-- BS + Minor -->
        <div class="container">
            <div class="subtitle">Tongji University</div>
            <div class="locale">Shanghai, CN</div>
        </div>
        <div class="container">
            <div class="emph">B.S. in Bioinformatics, Minor in Software Engineering</div> 
            <div class="timeline">Sep 2019 – Jun 2023</div>
        </div>
        <div class="bullets">
        <strong>Core Courses</strong>: Data Structures (C++), Machine Learning Theory, Software Engineering, Foundation of Database, Micro-service and Web Service, Calculus, Linear Algebra, Discrete Math, Numerical Methods and Algorithms
        </div>
    </section>
    <!-- Skill -->
    <section class="skills section">
        <div class="section-title">Skills</div>
        <ul class="bullets">
            <li><strong>Programming:</strong> Python, R, SQL, Java, C#, shell, HTML/CSS/JavaScript</li>
            <li><strong>Data Science:</strong> sklearn, PyTorch, TensorFlow, numpy, pandas, scipy, PySpark, HuggingFace, Transformers, PEFT, tidyverse, Shiny, Power BI</li>
            <li><strong>Concepts:</strong> Machine Learning, Deep Learning, Natural Language Processing, Computer Vision, Object-Oriented Programming, Data Structure, RESTful API, RDBMS, NoSQL, Agile Development, Cloud Computing, EC2</li>
        </ul>
    </section>
    <!-- Experience -->
    <section class="experience section">
        <div class="section-title">Experiences</div>
        <!-- AIQuraishi Lab -->
        <div class="container">
            <div class="subtitle">AIQuraishi Laboratory, Columbia University</div>
            <div class="locale">New York City, NY</div>
        </div>
        <div class="container">
            <div class="emph">Graduate Researcher</div> 
            <div class="timeline">Apr 2024 – Aug 2024</div>
        </div>
        <ul class="bullets">
            <li>Collected and tidied 600k+ peptide datasets and 35 protein datasets with Python to ensure high-quality data for model training and benchmark</li>
            <li>Trained transformer-based language models by masked sequence modeling on Slurm-supported HPC to generate protein representation</li>
            <li>Conducted benchmark pipeline with 5 models including Neural Network, Query Attention and Contrastive Learning with PyTorch Lightning</li>
        </ul>
        <hr>
        <!-- AI Engineer Intern -->
        <div class="container">
            <div class="subtitle">Radical AI Inc.</div>
            <div class="locale">New York City, NY</div>
        </div>
        <div class="container">
            <div class="emph">AI Engineer Intern</div> 
            <div class="timeline">May 2024 – Aug 2024</div>
        </div>
        <ul class="bullets">
            <li>Engineered a chat-based course assistant leveraging the Google Gemini model, displaying quiz generation and personalized learning instruction</li>
            <li>Established a robust FastAPI backend to process diverse files (YouTube videos, Microsoft documents, etc.) with LangChain and ChromaDB</li>
            <li>Ensured high performance through meticulous unit testing with Pytest and comprehensive integration testing within Docker environments</li>
        </ul>
        <hr>
        <!-- Data Engineer -->
        <div class="container">
            <div class="subtitle">Shanghai Foxhub Network Technology Company</div>
            <div class="locale">Shanghai, CN</div>
        </div>
        <div class="container">
            <div class="emph">Data Engineer Intern</div> 
            <div class="timeline">Aug 2022 – Oct 2022</div>
        </div>
        <ul class="bullets">
            <li>Formulated relational MySQL database architecture (ER diagrams) and managed unstructured data sources (OSS) on Alibaba Cloud</li>
            <li>Crafted shell scripts for database access permissions and backup operations, ensuring stability in production and development environments</li>
        </ul>
    </section>
    <!-- Proj -->
    <section class="projects section">
        <div class="section-title">Projects</div>
        <div class="container">
            <div class="subtitle">Custom LLM Chatbots with Character-Specific Tone</div>
            <!-- &nbsp; | &nbsp;  -->
            <!-- &nbsp;
            [<a href="proj-link">Details</a>] -->
            <div class="locale">Aug 2024 – Present</div>
        </div>
        <ul class="bullets">
            <li>Scraped 100+ collections of chat datasets from public wiki websites by operating a web scraper built with BeautifulSoup and Selenium in Python</li>
            <li>Fine-tuned 3 state-of-the-art LLMs like LLaMA leveraging LoRA technique on the HuggingFace/PEFT platform to tailor specific tone of chatbot</li>
            <li>Constructed RESTful API with FastAPI as backend and a multi-page app with Streamlit as frontend for interactive usage of customized models</li>
        </ul>
        <div class="container">
            <div class="subtitle">Billionaire Omics</div>
            &nbsp;
            [<a href="https://sitianzhou.github.io/BillionaireOmics/">Details</a>]
            <div class="locale">Nov 2023 – Dec 2023</div>
        </div>
        <ul class="bullets">
            <li>Arranged 4 modules to perform exploratory data analysis (EDA) with tidyverse to uncover patterns of 10-years billionaires assets dataset</li>
            <li>Formed a Shiny App with 3 panels for interactive data exploration featuring dynamic visualizations in longitudinal and geographic prospective</li>
            <li>Created 9-entries Bootstrap-based website on GitHub Pages, showcasing comprehensive findings and insights about billionaires worldwide</li>
        </ul>
        <div class="container">
            <div class="subtitle">Course Management System</div>
            <div class="locale">Nov 2022 – Jan 2023</div>
        </div>
        <ul class="bullets">
            <li>Led 4-members group to construct a micro-service system utilizing Java and React with engaging in agile development process including requirement specification, system design, implementation and testing, achieving a web service application with 4 main functionalities</li>
            <li>Built a hybrid database structure with MySQL for relational data and MongoDB for archival data maintained separately with 2 Docker containers</li>
            <li>Implemented and tested 34 RESTful APIs with SpringBoot framework and interactive website with React, Node.js, Axios, Bootstrap, Webpack</li>
        </ul>
        <div class="container">
            <div class="subtitle">Neurodegenerative Diseases Onset Prediction</div>
            <div class="locale">Jun 2022 – Jul 2022</div>
        </div>
        <ul class="bullets">
            <li>Completed data collection and feature engineering on open-source patient data about the onset of Alzheimer’s disease and Parkinson’s disease</li>
            <li>Launched predictive models achieving 80%+ accuracy based on SVM, decision tree via sklearn and provided Flask website for interactive usage</li>
        </ul>
        <div class="container">
            <div class="subtitle">PlantDB Desktop App</div>
            <div class="locale">May 2022 – Jun 2022</div>
        </div>
        <ul class="bullets">
            <li>Delivered desktop app with 13 interactive interfaces and 3 roles for plant information retrieval and note-taking based on C# and VS.NET</li>
            <li>Devised and deployed a relational database on SQL Server platform to set up schema for user accessibility, note storage and plant searching</li>
        </ul>
        <div class="container">
            <div class="subtitle">Neurodegenerative Diseases Onset Prediction</div>
            <div class="locale">Jun 2022 – Jul 2022</div>
        </div>
        <ul class="bullets">
            <li>Collected and processed 36 solid waste datasets from Zhejiang Province to establish a robust foundation for model training and analysis</li>
            <li>Realized about 21% increase on metrics like Pearson coefficient of solid waste composition prediction task using neural network model with machine learning technologies including L2 regularization, Adam optimizer and dropout, batch normalization via PyTorch library</li>
            <li>Visualized data features and model evaluation results via matplotlib and Tensorflow library to give instructions for garbage processing schedule</li>
        </ul>
    </section>