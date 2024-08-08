<h1>Smile coach AI</h1>
This model categorizes human expressions, some are negative, some are positive. Sometimes the inexplicable bad mood may be due to the dull expression,  This model will remind you at these times. Smile, and harvest a good start.
<h1>The Algorithm</h1>
The model was created based on jetson nano and trained on datasets of smiles and non-smiles
<h1>Running this project:</h1>
Change directories into jetson-inference/python/training/classification/data



[smile_non.zip](https://github.com/user-attachments/files/16533048/smile_non.zip)

 Run this command to download the dataset. 
 ```
wget https://github.com/user-attachments/files/16533048/smile_non.zip
```

Run this command to unzip the file you downloaded. 

```
unzip smile_non.zip -d jetson-inference/python/training/classification/data
```

Once that has run and you're still back in the jetson-inference folder, run
```
./docker/run.sh
```

From inside the Docker container, change directories so you are in jetson-inference/python/training/classification

run
```
python3 train.py --model-dir=models/smile_non data/smile_non
```

While it's running, you can stop it at any time using Ctl+C. You can also restart the training again later using the --resume and --epoch-start flags, so you don't need to wait for training to complete before testing out the model.

if allocate memory is displayed,enter it: 
```
echo 1 | sudo tee /proc/sys/vm/overcommit_memory
```

Make sure you are in the docker container and in jetson-inference/python/training/classification
 
Run the onnx export script.
```
python3 onnx_export.py --model-dir=models/smile_non
```


Exit the docker container by pressing
Ctl + D.

Set the NET and DATASET variables
```
NET=models/smile_non
DATASET=data/smile_non
```


Run this command to see how it operates on an image from the smile folder.
```
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/Aaron_Guiel_0001.jpg  output1.jpg

imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/Abdel_Aziz_Al-Hakim_0001.jpg  output2.jpg

imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/Ahmed_Ahmed_0001.jpg  output3.jpg

```

Then you can view the results

![test1](https://github.com/user-attachments/assets/056ca6cb-8513-44b4-90ac-33b074fdcd53)


![text2](https://github.com/user-attachments/assets/060b1f1f-ef31-4f07-9cc5-ac14d442d059)


![text3](https://github.com/user-attachments/assets/2405d509-69c5-4e15-a211-fa9bd40d6518)
