# Data_Science_Assessment

This is the repository that contains the solutions to the Data Science Assessment assignment. 

### Exercise 1

For exercise 1, the classification problem was chosen: the task is to implement a binary classifier. The code can be found in the file named 'Exercise_1.ipynb'. 

## Technical Questions

### Exercise 2

2.1	Can you explain the purpose of the orders_items table? 

The orders_items table is used to log the items present in each order. The orders table contains information about each order, but no information about what items are in that order. Similarly, the products table contains information about the products, but not about who ordered them, what order they are part of or in what quantity. Thus, the orders_items table serves as a 'bridge' between the two tables and contains more specific information about how many items of a product are in a certain order. This information is necessary to process the order, because it will show what specific items belong to an order. 

2.2	Can you write a SQL query to find the average order cost per country?

SELECT AVG(orders.order_cost) 
FROM customers, orders
WHERE customers.country = country AND orders.id_customer = customers.id


2.3	Can you write a SQL query to find the name of the highest price product sold to an Italian customer?

SELECT products.name, MAX(products.price) 
FROM customers, orders, orders_items, products 
WHERE customers.country = 'Italy' AND orders.id_customer = customers.id AND orders_items.id_order = orders.id AND products.id = orders_items.id_product

2.4	How could you optimize the orders table assuming you have to manage a lot of queries?

2.5 What would you change if the amount of data is too big to run these queries? (suppose to have 500TB of data)
If the amount of data was too big to run the queries, I would remove some redundant information contained in the tables, e.g. the name of the product in the 'products' table, which is not necessary in processing the order as well as the date of birth of the customer (admittedly, this information could be useful for market analysis, but it is not necessary to process the order). I would also merge the firstname and lastname columns in the 'customers' table, which would allow to more quickly access the full name of a customer. The 'id' column in the 'orders_items' table is also not being used in other tables, so it might not be necessary to identify the order. 



### Exercise 3

3.1	How would you design a data pipeline for a Machine Translation system? (e.g. necessary steps, main challenges, etc.)

In a Machine Translation pipeline it is first necessary to prepare the data: to clean the text (split the text in a way to have parallel phrase pairs in the source and target languages, eliminate unwanted punctuation etc.). Then the data has to be split into train, validation and test sets. Then the model has to be trained (I would choose to use an encoder - decoder architecture with a self - attention mechanism, since they allow for variable length input – output sequences and the attention mechanism is useful for long sequences of text, since it helps the model to learn where to place attention when encoding and decoding the sentences). After training, the model can be evaluated on the validation set and, based on its performance some hyperparameters can be changed to improve it. Afterwards it can be evaluated on the test set and launched. 
Some challenges can could arise during the cleaning of the data: it can be a long and tedious process. Another challenge is given by the training times: it can take a long time to train such models (which is expensive, especially if the model has to be retrained), especially when there is a lot of training data and it is trained for a large number of epochs. The challenge is thus finding the optimal number of epochs for training in order to still get good performance while reducing training times.


3.2 What would you do to collect new and good quality data from the web? Assume you want to use them to train a neural model for Machine Translation.

To collect new and good quality data: first of all, I would consider the target domain of the translation: is the model going to be used in a specific context, to translate texts of a specific genre or if it is a general purpose MT model. If the model is going to be used in a specific context (e.g. scientific literature), it would be best to train it on texts of such nature. If the model is going to be deployed on various texts of different genres, then I would try to create a set of diverse genres for training, so that the model does not overfit to one specific genre (or style/topic) and is able to generalize well. To find the data I would first of all search if there are any parallel corpora readily available on the internet. Ideally, I would search for original texts in the source language, written by native speakers with translations produced by native speakers of the target language. 
If no professional translations can be found on the web, then I would search for websites that have content in both the source and target languages. These websites could then be crawled and the sentences could be automatically paired (the data from website crawling will of course need to be appropriately cleaned and validated to make sure that the quality of the translations is good). 


3.3 Suppose that low quality translations created by the system are post-edited by professional translators. How would you use this process to monitor the quality of the Machine Translation system? 

In order to monitor the quality of the Machine translation system by using corrections made by professional translators it is important to see what type of corrections are being made, so to understand what kind of mistakes the model is making. Various types of mistakes are possible in a translation: on a syntactical or semantic level or simply mistakes regarding language style and register. A good understanding of where the translation goes wrong can be of help in improving the model. If the translators mainly modify the style of the translation, this might mean that the model does not generalize well to different language registers, so it might either be overfitting the training data, or it might have been trained on very specific data (translations). If there are many grammatical errors, it is likely that the syntactical structure, i.e. the relationships between words in a sentence, of either the source or the target language, or both, is not captured well by the model. This might then be improved by modifying the architecture of the model (by adding more layers, adding self-attention or in other ways, depending on the initial model). Finally, semantic errors will likely indicate that the system does not “understand” (i.e. encode well) the meanings of words, so during the translation it chooses words that do not have the same meaning in the source and target languages. A possible way to improve the model’s performance in this case could be increasing the size of word embedding vectors. 