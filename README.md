Vehicle Verifier Model

This model is used to identify different vehicles as a bus, plane, bike, or boat. It is trained on an imagenet Resnet-18 model using transfer learning.

![image](https://github.com/HalenSa/Vehicle-differentiator/assets/140643980/58ccf0f9-c32d-4980-ba03-f8cb3270a4be)

![image](https://github.com/HalenSa/Vehicle-differentiator/assets/140643980/2cbb39e5-acb0-4b7b-828e-fc650c887f41) 

![image](https://github.com/HalenSa/Vehicle-differentiator/assets/140643980/51654ce7-c965-4b6e-b1fb-5e1ddb247cc0)



## The Algorithm
The algorithim is used by showing an image of one of the four vehicles to the Jetson nano. It uses a 2GB Jetson Nano, and so it uses it a preflashed SD card flashed from the NVIDIA webpage. It identifies the vehicle shown using the references in the dataset with imagenet. Afterwards it will print a percentage of accuracy to the vehicle as the type it identified it as. When running the command imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/(image class)/(image name).jpg (name of output file).jpg an image is returned with the original image of the vehicle along with a percentage of accuracy as shown above.
Note: This was run at 1000 epochs for training but the dataset is small so accuracty may be reflected as a smaller percentage than expected.
## Running this project
You can download the dataset with this link: https://www.kaggle.com/datasets/rishabkoul1/vechicle-dataset
1. Connect to your Jetson Nano via VSCODE. 
2. Click on the small green icon at the bottom left of your screen to access the SSH menu.
3. Select + Add New SSH Host to add a new host.
4. Enter ssh nvidia@x.x.x.x, replacing x.x.x.x with the IP address you usually use in Putty or terminal to connect to the Nano.
5. Pick the first configuration file.
6. Click Connect in the prompted window.
7. Choose Linux as the operating system when asked.
8. If you're asked to continue, click Continue.
9. You'll be asked for a password after connecting to the Nano. Input your Nano password and hit Enter.
10. Select Open Folder and navigate to jetson-inference. Input your password again if required.
11. Click Yes, I trust the authors
12. Navigate to jetson-inference/python/training/classification/data.
13. Extract the dataset ZIP file.
14. Inside jetson-inference/python/training/classification/data, create a new folder called vehicles. Inside vehicles, add three folders: test, train, val and a file named labels.txt.
15. In the train directory inside fruits, create 4 folders named bike, boat, bus, and plane
16. Copy these folders to the val and test directories.
17. Distribute the images from your ZIP file among these folders, with 80% in the train folder, 10% in the val folder, and 10% in the test folder for each fruit type. Unfortunately, this will be a manual task and may take some time.
18. Go to the jetson-inference folder and run ./docker/run.sh.
19. Once inside the Docker container, navigate to jetson-inference/python/training/classification.
20. Run the following command: python3 train.py --model-dir=models/vehicles data/vehicles --epochs=(amount of epochs you want, 1000 is optimal but will likely need to be run overnight)
21. Navigate to jetson-inference/python/training/classification while still in the Docker container.
22. Run the ONNX export script: python3 onnx_export.py --model-dir=models/vehicles
23. Exit the Docker container by pressing Ctl + D in the terminal.
24. On your Nano, navigate to jetson-inference/python/training/classification.
25. Check if the model exists on the Nano by executing ls models/vehicles/. You should see a file named resnet18.onnx.
26. Set the NET and DATASET variables: NET=models/vehicles DATASET=data/vehicles (as seperate commands)
27. Run the command: imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/(image class)/(image name).jpg (name of output file).jpg
28. Afterwards you will have your output image with a percentage just below vehicles. Run this command as many times as you want to test the accuracy of the nano and adjust the amount of epochs in the previous step as you wish (make sure to follow every step past the training again after adjusting the epochs)

![View a video explanation here]((https://www.youtube.com/watch?v=WoQ-ObMadJc)
