<objective>
The objective is to write the methodology section of a research paper showing this deep learning method of decoding tri-axial accelerometer and tri-axial magnetometer data from sheep animal movement into a set of animal behaviours. 
The method uses a convolutional neural network (CNN) together with a transformer model to decode the data, which is processed into sequences of segments, where a segment can be powers of 2 seconds, and the sequences can have more or less number of segments. We use the convolutional neural network to make an initial (sortof isolated) classification of the segment, and then the we use the transformer model to update the classification of the segment given all the other segments of the sequence, somehow incorporating the time series aspect of the data (e.g., if an animal is resting for many segments, probably it will be resting during the next or previous segment). 

The structure of the methodology section of the research paper should at least have the following sections:
- Data 
- Model 
- Training

Add more sections or subsections if you consider it necessary or desirable. 

</objective>



<key_principles> 
- when writing, every paragraph's starting sentence should be a single sentence that is a summary of the paragraph. before writing a paragraph, write the first sentence and think if that summarizes the rest of the yet to be written paragraph. 
- when writing, keep the signal/noise ratio very high, don't write gibberish and don't write anything that is not necessary to fill up the text. 
- when writing, keep the tone professional but with the main goal of being understandable and not to impress. 

- MOST IMPORTANT ONE: when writing about things like the data or the model, don't write anything that you are not sure about and you have the data to back it up. e.g. you need to be sure on every aspect such as data wrangling approach, number of samples/individuals, etc. If you are not sure, just place a <not_sure> tag and I'll help you get the data to back it up. 

</key_principles>



<sources>
This repository does the data extraction and processing, the model definition, model training, and model evaluation. 

the `scripts` folder contains the main code for doing everything, but does not contain a main.py file where it does the whole process. 
How the whole process can be derived from the jupyter notebook at `analysis/sheep_decoder_TF_CLEAN.ipynb`

there are other folders that can contain useful information, but always check with the human before diving into a file because it might not be usefull information and that will only add noise to our context. 

</sources>