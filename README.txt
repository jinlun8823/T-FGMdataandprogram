File description:

	This document is the related procedure for the paper "Trustworthy Flight Guidance Method for Civil Aircraft Autonomous Operation Based on 4D Trajectory - A Case Study of COMAC C919". We provide the main original data, Python code files, as well as related graphics and simulation programs in these files.

	(1) File "1.OriginalATCdataAndProcess" is the text of the original controller's instruction data, and the corresponding data processing programs.

	(2) File "2. SetorsAndAirroutes of China"  includes the data of China's control areas, air-routes, and the corresponding decoding and visualization programs.

	(3) File "3.Implementation of Bert LSTMain"  is the main Bert LSTM method for model training and generating flight guidance instructions.

	(4) File "4.Trajectory Feature Cognition" is a program for feature recognition of four-dimensional trajectory data.

	(5) File "5.SimpleSimulatorforAircraftControl" is a simple aircraft simulation control model designed to provide a platform for scholars who do not have a simulator to guide and control aircraft simulation. The simulation of human-machine interaction control of the aircraft can be achieved through simple keyboard keys

	(6) File "6. DrawFiguresinHorizontalVertical is the main trajectory visualization program

	(7) File "7. Calculated Errors is a program for calculating trajectory control errors

	In each subfile, there is a corresponding README description too, which includes the descriptions of the relevant data, functions, program execution order, and the output results.


	(1) Description of file "1.OriginalATCdataAndProcess" :

	The file "originalATCdata.txt" is the original control instruction data, which comes from various control areas in China. We manually removed some instructions about holding, diversion, landing/takeoff permits, or non-standard phonetic instructions to reduce the scope of flight guidance appropriately.

	It should be noted that some controllers send control instructions in the form of "[callsign], [XXX control] (name of the control unit), XXXXXX (instruction)". We have removed [XXX control] (name of the control unit) because the flight guidance method proposed in the paper is deployed onboard the aircraft, not provided by specific control units. Of course, this is also due to the confidentiality requirements of civil aviation.

	Firstly, "dataProcess(classified).py" is used to classify the control instructions in "originalATCdata.txt". If you successfully execute this file, we will have files as follows:
"climb_commands.txt","descend_commands.txt","increase_speed_commands.txt","maintain_commands.txt","reduce_speed_commands.txt","turn_commands.txt","waypoint_time_commands.txt",
The function of this program is to classify control instructions according to their control purposes, such as turning, climbing, descending, accelerating, decelerating, etc.

	Then execute the program "dataProcess (label).py" to label the text data, and the output result is "processed_file_with_labels.txt". This file completes the labeling of the  control instruction text, and the effect is as follows:

	"TAM213, turn left heading 170."   ==> [CLS] [OBJ]TAM213 [SEP] [REQ] turn left [ACTION] heading [STA]170 [CLS]
	"ANA567, descend to FL 177."  ==> [CLS] [OBJ]ANA567 [SEP] [REQ] descend [ACTION] to [STA]FL 177 [CLS]
	"SIA783, climb to reach 9200 meters by LKO." ==> [CLS] [OBJ]SIA783 [SEP] [REQ] climb [ACTION] to [STA]reach 9200 meters [ACTION] by [STA]LKO [CLS]

	The data obtained after labeling can be used for further Bert vectorization and LSTM model training.


	(2) Description of file "2.SetorsAndAirroutesofChina":

	This is the data of China's national control areas and airspace routes, with the original data files"sector. xml" and "airway. xml".

	"read_sector_airway. py" is a Python decoding program for XML files.

	"SegmentandAirroutesPlot.py " is a visualization program of aerospace sectors and routes data.

	Certainly, this is the old data of China's airspace in 2017, not the current airspace structure operating in China. And some biases have been applied to some critical routes, waypoints, and sectors. Therefore, there is no risk of confidentiality, which can be used to support academic research conveniently.

	For example, flight plan of the aircraft can be set based on the positions of the route points.


	(3) Description of file "3.Bert-LSTMmain":

	This is the main program for implementing the Bert word vectorization and LSTM flight guidance instructions generating model.

	Firstly,  the file "processed_file_with_labels.txt" is the result of the previous processing of 1. OriginalATCdataAndProcess, and it is also the input data here.

	Execute 'BertTokenEmbedding.py' to vectorize the tagged text data. If executed successfully, we will have:
	"sentence_embeddings.xlsx" and 'sentence_embeddings.npy"
	These two files store the data processed by Token-Embedding procedure, which is also the training data for the subsequent LSTM model.

	Then execute the 'LSTModelTraining.py' file to train the flight guidance instruction generating model. If the execution is successful, we will obtain file:
"lstm_model.pth"

	In this file, "TrajectoryFeatureCognition.py" is the 4D trajectory feature recognition module used to assign values to the [STA] position for the flight guidance instructions generated by the LSTM.


	(4) Description of file "4.trajectoryFeatureCognition":

	This is a feature recognition program for 4D trajectory data, which takes a discrete set of 4D trajectory data points as input and outputs the trajectory features of each point, such as "climb", "descent", "left turn", etc.

	The usage of 'trahectoryFeatureCognitionRBF.py' is similar to 'trajectoryFeatureCognitionMathmatically.py', but the inherent methods are different. The former is based on RBF neural network, while the latter is based on partial derivative and curvature calculation.

	
	(5) Description of file "5.SimpleSimulatorforAircraftControl":

	This is a simulation program that simulates aircraft control. We can achieve aircraft simulation control through the keyboard and display the aircraft's operating status in Python matplotlib.

	Execute the main program "AircraftSimulator.py" on the Python terminal to start simulating flight. Of course, before that, you can customize the flight plan route in the program.

	Simulate airplane control through keyboard keys:

	Up arrow ↑: Small slope climb
	Down arrow ↓: Climbing on a small slope
	Left Arrow ←: Small Slope Left Steering
	Right Arrow →: Small Slope Right Steering
	A: Acceleration
	D: Slow down
	L: Turn left on a steep slope
	R: Turn right on a steep slope
	Continuously press the up arrow ↑↑: climb on a steep slope
	Continuously pressing the arrow ↑↑: descending on a steep slope
	Press ESC twice to exit the simulation software (if it doesn't work, try closing the Python console)

	It is a relatively simple software for simulating pilots controlling aircraft, mainly used to support scholars who do not have flight simulation cabins to conduct simulation control experiments, whose requirements for trajectory execution accuracy are not so strict, and also to make it easier for researchers to understand the content of the paper.


	(6) Description of file "6.DrawFiguresinHorizontalVertical":

	Here are some drawing programs used to plot trajectory images (Lateral, longitudinal, and vertical).


	(7) Description of file "7.CalculateErrors":

	This is a program used to calculate trajectory execution error.


文件说明(中文)：

	这份文件是论文《Trustworthy Flight Guidance Method for Civil Aircraft Autonomous Operation Based on 4D Trajectory--A Case Study of COMAC C919》的实验程序，我们提供了主要的原始数据、python代码文件，以及相关的绘图、仿真程序。

	文件"1.OriginalATCdataAndProcess"为原始管制语音指令数据，以及对应的数据处理程序

	文件"2.SetorsAndAirroutesofChina"为中国管制区结构、航路航线数据，以及对应的解码、可视化程序

	文件"3.Bert-LSTMmain为主要的Bert-LSTM"生成飞行引导指令的方法实现

	文件"4.trajectoryFeatureCognition"为四维航迹数据特征识别的程序

	文件"5.SimpleSimulatorforAircraftControl"为简易的航空器仿真控制模型，旨在为没有模拟机的学者们提供一个航空器仿真引导控制的平台，通过简单的键盘按键即可实现航空器的人机交互控制仿真

	文件"6.DrawFiguresinHorizontalVertical"为主要的航迹可视化程序

	文件"7.CalculateErrors"为计算航迹控制误差的程序

	在每个子文件中，有对应的README说明，包含相关数据、功能、程序执行顺序、执行输出结果的说明。
