![header](https://capsule-render.vercel.app/api?type=waving&color=random&text=Slot-Tagging&animation=fadeIn&fontColor=B5B5B6)

<h5 align='center'> Tech Stack </h5>

<p align='center'>
  <img src="https://img.shields.io/badge/Python-3766AB?style=flat-square&logo=Python&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=Jupyter&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/Colab-F9AB00?style=flat-square&logo=Google Colab&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/Flask-000000?style=flat-square&logo=Flask&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/Selenium-43B02A?style=flat-square&logo=Selenium&logoColor=white"/></a>&nbsp
  <img src="https://img.shields.io/badge/Numpy-013243?style=flat-square&logo=Numpy&logoColor=white"/></a>&nbsp
</p>

# Coffee Order Chatbot 
### - Utilizing BERT fine-tuning for slot tagging

***(Not supported in TensorFlow 1.x as of now 25.01.05)***
  
1. Export Pretrained BERT Model as a Module

    - This involves creating a BERT module from the checkpoint of a pretrained KorBERT model by ETRI
    - `python export_korbert/bert_to_module.py -i {checkpoint_directory} -o {output_directory}`   
    - Example: `python export_korbert/bert_to_module.py -i /content/drive/MyDrive/004_bert_eojeol_tensorflow -o /content/drive/MyDrive/bert-module`  


  
2. Data Preparation

    2.1 Creating Required Files : `seq.in` and `seq.out`

     - Convert input data into format required for training, creaing `seq.in`(input sequences) and `seq.out`(output labels).
   - Command : `python prepare_data.py process_file({input_data_path}, {output_path}), process_line(sentence, tokens)`
   - Example : If the input is "라떼"(latte in Korean), it will be tokenized into "라" and "떼" for more accurate classification.
      
    2.2 Splitting Data into Train, Validation and Test Sets

   - Split the `seq.in` and `seq.out` files into train, validation, test sets in an 8:1:1 ratio.
   - Command : `python split_new.py`



3. Fine-Tuning the Model
   
   - Train the model using the prepared datasets.
   - Command : `python train.py -t {train_set_directory} -v {validation_set_directory} -s {model_save_directory} -e {num_epochs} -bs {batch_size}`
  
4. Model Evaluation

   - Evaludate the trained model using the test dataset.  
   - Command : `python eval.py -m {trained_model_directory} -d {test_set_directory}`
   - The test results will be saved in the `test_results` folder under the specified model directory
  
6. Inference 

    - Use the trained model to make predictions on custom input sentences.
    - Command : `python inference.py -m {trained_model_directory}`  
    - Example: `python inference.py -m saved_model/`   
        - When "Enter your sentence:" appears, enter the sentence you want to test in the model.
        - To exit: enter 'quit', '종료', '그만', '멈춰', or 'stop'.



----------------------------------------------------------------------------------------------------------------------------------------------------
## Project Overview
### Coffe Order Chatbot
A chatbot based on the Starbucks menu that allows users to order beverages and food.

***Note: The images included in this documentation are written in Korean.***

![image](https://user-images.githubusercontent.com/86215518/143837501-67352039-9096-482b-92de-962c4f45fe11.jpg)



### 01. Purpose of Development
- The exisiting Starbucks Korea's Siren Order app is inconveninent and difficult to use, which motivated the development of an easier and more accessible chatbot service.
![image](https://user-images.githubusercontent.com/86215518/143963783-573a9b96-a283-4fd1-bd5f-4cbe89b0df6d.jpg)




### 02. Service Scenarios
- __Objective__: To accurately idenfity cusomer orders using keyword extraction. 
    - Example:
        - "아메리카노" (Americano), "라떼" (Latte) -> __음료 (Drink)__
        - "쿠키"(Cookie), "케이크"(Cake) -> __음식 (Food)__
        - "따뜻하게"(Hot), "차갑게"(Cold) -> __온도 (Temperature)__
![image](https://user-images.githubusercontent.com/86215518/143963963-c0a4ab21-d0bd-4377-b0de-9c6be1d91c7c.jpg)

#### Example
(Translation)

    - "One grande cappuccino (beverage), hot, please. Also, please give me a raisin cookie (food)."
     "Is there anything else you need?"
    - "No, that's all."
     "Your order will be one grande hot cappuccino and a raisin cookie."
![image](https://user-images.githubusercontent.com/86215518/143965388-fcab1642-b576-418c-98ce-10f45d21f671.jpg)

#### Exception Handling Scenario
- Planning for cases where empty slots or exceptions occur (e.g., unrecognized orders) and defining how the chatbot should respond.
  
(Translation)

    - "One grande mango banana blended, please. Is it possible to add bubble tea?"
     "Unfortunately, that option is not available."
    - "Why not?"
     "I'm sorry, but it's an unavailable option."

![image](https://user-images.githubusercontent.com/86215518/143964415-28852eb3-e3d0-4c14-8598-69764f438fbd.jpg)




### 03. Fine-Tuning Method
#### Base Model : BERT (bidirectional representations from transformers)
- We fine-tuned a pretrained KorBERT(Korean BERT) to suit the purpose of coffee order chatbot.
![image](https://user-images.githubusercontent.com/86215518/143964504-6ca65eae-3fdb-4f44-b456-541d1218b6fb.jpg)

#### Slot Tagging
- Slot tagging is the process that assigns labels to specific words or phrases in a sentence allowing machines to understand the roles of words within sentences.[(ref: Wikipedia)](https://en.wikipedia.org/wiki/Semantic_role_labeling#:~:text=In%20natural%20language%20processing%2C%20semantic,the%20meaning%20of%20the%20sentence.)
![image](https://user-images.githubusercontent.com/86215518/143964574-87b4def6-6981-42c3-80be-5665d5244ea5.jpg)



### 04. Data Collection Strategy
- Web scraping of the Stackbuks Menu
- Expanding data with different sentence structures using loop statements
- Designing an ordering scenario using condifional statements
- Exception handling for edge cases
![image](https://user-images.githubusercontent.com/86215518/143964886-bdb217db-0460-4bb2-9090-a281e14bbea5.jpg)



### 05. Service Development Plan
#### Development Steps
- Design order scenarios → Build the dataset → Train the model (Fine-tuning) → Develop the web service → Integrate and deploy the model with the web
![image](https://user-images.githubusercontent.com/86215518/143964983-446a9da5-1162-4fe7-9a66-016a05ab905e.jpg)

#### Implementation
: Used Flask, Figma, HTML, and CSS to build a web-based chatbot system.
![image](https://user-images.githubusercontent.com/86215518/143965023-e2920c86-c6ed-4f81-9c42-1de3ce4f36e7.jpg)

#### Demonstration
![image](https://user-images.githubusercontent.com/86215518/143965045-afac7883-0313-4d32-8b51-7ad082c075f5.jpg)



----------------------------------------------------------------------------------------------------------------------------------------------------
### Our Team
##### [Seul Yi](https://github.com/seuly1203) (Team Leader) : Chatbot Algorithm Design and BERT Fine-tuning for Slot Tagging

##### [Park Mina](https://github.com/parkmina365) : Chatbot Algorithm Design and BERT Fine-tuning for Slot Tagging

##### [Jung Harim](https://github.com/hharimjung) : Web Development and Presentation Design

##### [Sumun Oh](https://github.com/sumunoh) : Web Scraping and Development

##### [Incheol Shin](https://github.com/InChil2) : Technical Lead



  
