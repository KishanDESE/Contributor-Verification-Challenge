1. Preprocessing (Inside folder PreProcessingCSV)
	1.1 Generating PMI numpy arrays (Inside folder PMICandidateMatrixCreate)
		PMIMatrixSaveCandidateCoauthor.py
		YearCandidatePMICreate.py
		PMICreateCandidateVenue.py
	1.2 Generate clustering using LDA
		LDARemote.py: Generate LDA 
	1.3 Generate Training Features: (Inside folder TrainCSVCreater)
		LDACallFunction.py : Call LDA clustering saved model for getting cluster index and probability of cluster
		Graph_Builder.py : Generate knowledge graph between candidate, topic, venue
		PMICall.py : Calling PMI saved numpy array from step 1.1
		NodeVec.py : Generating TF-IDF graph for contributor-contributor
		ClTextCandidate.py : Generating Word2vec for text
		ClCoAuthorCount.py : Adding feature number of counts given candidate-contributor written article together
		ClArticleNo.py : Adding feature number of articles written by candidate
		CSVGenerator.py : Generates CSV by calling function from above python files
		TopicPMIAdd.py : Adding feature PMI between candidate and topic
		Similiar steps for test features generation.

2 Training Random Forest (Inside folder TrainRF)
	2.1 Train Model for candidates having atleast 1 contributor: (Inside folder RFTrainWithAtleast1Contributor)
		2.1.1 Train Model with all features (Inside folder RFTrainWithAllFeatures)
			TrainRF.py
			TestRF.py
		2.1.1 Train Model with low importance feature (feature with no contributor knowledge) features (Inside folder RFTrainWithAllFeatures)
			TrainRF.py
			TestRF.py
	2.2 Train model for candidates having 0 contributor in test.json: (Inside folder RFTrainWith0Contributor)
		TrainRF.py
		TestRF.py

3. Correct Misclassifications: (Inside folder MisclassifiedCorrection)
   RankMisclassified.py

4. Ensemble Voting: (Inside folder VotedPrediction
   Voting.py

FEATURES:
- Graph relationships and paths
- PMI statistics between entities
- LDA topic modeling
- Text semantic similarity
- Contributor network features

MODEL: Random Forest classifier for contributor identification

PURPOSE:

Part 1: Feature Generation
-Generates PMI feature numpy array to call 
-Generate LDA clustering
- Extracts multiple feature types from data including TF-IDF graph and knowledge graph relationships, PMI scores, and content features

Part 2: Standard Model Training  
- Trains Random Forest on cases with multiple contributors using complete/low important feature set
- Trains separate Random Forest optimized for zero contributor in test.json

Part 3: Prediction Correction
- Corrects misclassified "0" predictions from merged model outputs
- Uses alternative predictions from model trained on reduced (low importance/ no contributor knowledge) features
- Generates final submitted CSV with improved "1" prediction accuracy

Part 5: Ensemble Voting
- Combines predictions from multiple models (SVM, XGBoost, Random Forests) using weighted voting
- Weights are based on individual model accuracies (72%, 75%, 77%, 87%, 88%)
- Generates final ensemble predictions through probability-weighted averaging
- Outputs "Voting_prediction.csv" with improved overall performance
