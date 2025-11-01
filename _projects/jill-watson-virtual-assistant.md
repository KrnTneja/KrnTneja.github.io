---
title: "Jill Watson: An LLM-based Virtual Teaching Assistant"
excerpt: "A conversational AI teaching assistant that enhances teaching and social presence in online learning environments"
collection: projects
technologies: "OpenAI LLMs, Skill-based Architecture, Learning Analytics, VectorDBs, Learning Management Systems, Learning Tools Interoperability"
impact: "Presented at AIED 2024 and Learning@Scale 2024, improved teaching presence in classrooms, supported thousands of students across multiple courses"
header:
  teaser: /images/jill-watson-teaser.png
date: 2025-10-28
---

## Overview

Conversational AI agents often require extensive datasets for training that are not publicly released, are limited to social chit-chat or handling a specific domain, and may not be easily extended to accommodate the latest advances in AI technologies. 
In our paper at [AIED 2024](https://link.springer.com/chapter/10.1007/978-3-031-64302-6_23), we introduced Jill Watson, a conversational Virtual Teaching Assistant (VTA) leveraging the capabilities of ChatGPT. 
Jill Watson based on ChatGPT requires no prior training and uses a modular design to allow the integration of new APIs using a skill-based architecture inspired by XiaoIce. 
Jill Watson is also well-suited for intelligent textbooks as it can process and converse using multiple large documents. 
We exclusively utilize publicly available resources for reproducibility and extensibility. 
Comparative analysis shows that our system outperforms the legacy knowledge-based Jill Watson as well as the OpenAI Assistants service. We employ many safety measures that reduce instances of hallucinations and toxicity. 
The research work also includes real-world examples from a classroom setting that demonstrate different features of Jill Watson and its effectiveness.

![Jill Watson Virtual Teaching Assistant](/images/jill-watson-teaser.png)
*Figure from [AIED 2024](https://link.springer.com/chapter/10.1007/978-3-031-64302-6_23)*

Our paper at [ITS 2024](https://link.springer.com/chapter/10.1007/978-3-031-63028-6_7) shows how courseware like textbooks, user guides, video lesson transcripts, course websites, and class syllabi have been used with Jill Watson in several classes with close to 1300 students at the Georgia Institute of Technology as well as in two community colleges in the Technical College System of Georgia. We found that Jill Watson enhances the positives of conversational courseware (such as answering questions and engaging in conversations anytime and anyplace) and suppresses the negatives of large language models (such as biases and hallucinations).

![Jill Watson Usage as of Spring 2024](/images/jw-stats-spring-24.png)
*Jill Watson Usage as of Spring 2024 (Figure fromm [ITS 2024](https://link.springer.com/chapter/10.1007/978-3-031-63028-6_7))*

As online learning at scale has become dramatically more popular over the last decade, these programs provide affordable and accessible education, but low retention and engagement are persistent problems. 
Virtual Teaching Assistants (VTAs) such as Jill Watson offer a solution as they can answer questions about course logistics and content, amplifying interaction between professors and students, increasing teaching presence, and thereby improving retention and engagement. 
Using the Community of Inquiry framework, our paper at [L@S 2024](https://dl.acm.org/doi/abs/10.1145/3657604.3664679) presented what we believe is the first experimental study of the effect a VTA has on student perceptions of teaching presence, social presence, and cognitive presence. 
Students in a large, online, graduate computer science course were randomly assigned to sections with and without access to Jill Watson. 
The Community of Inquiry survey was then administered at the end of the semester to measure the three presences. 
We found that Jill Watson has a small, positive, statistically significant effect on the Design & Organization dimension of teaching presence as well as social presence.

### Deployment of MuDoC as Jill Watson

[MuDoC](/projects/multimodal-document-grounded-ai/) has been deployed as Jill Watson in  [Prof. David Joyner](https://www.davidjoyner.net/)'s Fall 2025 [Knowledge-based AI course](https://lucylabs.gatech.edu/kbai/) in Georgia Tech's OMSCS program. Read the [MuDoC Project Page](/projects/multimodal-document-grounded-ai/) or watch the intro video below!

<iframe width="560" height="315" src="https://www.youtube.com/embed/Et_y8d8HAV8?si=rGGg-anFr96kKT_H" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Related Publications 

- [AIED 2024](https://link.springer.com/chapter/10.1007/978-3-031-64302-6_23)
- [ITS 2024](https://link.springer.com/chapter/10.1007/978-3-031-63028-6_7)
- [L@S 2024](https://dl.acm.org/doi/abs/10.1145/3657604.3664679)

### Team

Karan Taneja (Development Lead), Pratyusha Maiti, Sandeep Kakar (Deployment Leads), Robert Lindgren, Pranav Guruprasad, Sanjeev Rao, Alekhya Nandula, Gina Nguyen, Aiden Zhao, Vrinda Nandan (Developers), Ashok Goel (Advisor)