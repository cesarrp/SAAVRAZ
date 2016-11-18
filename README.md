Using pyAudioAnalysis with some modifications for everything but audio segmentation
*pyAudioAnalysis wiki. Click [here] (https://github.com/tyiannak/pyAudioAnalysis/wiki) for the complete wiki*

Full script so far (sample/analysis_tipe.txt):

for file in ../pt_data/*.wav; do filename=$(basename "$file"); filename="${filename%.*}"; auditok -i $file -o $filename"_{N}_{start}_seg.wav"; done;

for file in *.wav; do
echo "file spectogram"
python ../audioAnalysis.py fileSpectrogram -i $file;

echo "python ../audioAnalysis.py trainClassifier -i <directory1> ... <directoryN> --method <svm, knn, extratrees, gradientboosting or randomforest> -o <modelName> --beat (optional for beat extraction)"

echo "create classes for regression (-o: output)"
echo "python ../audioAnalysis.py trainRegression -i ../pt_data/ --method svm -o ../data/svmSpeechEmotion"

echo "classifying into classes in trained regression"
echo "python ../audioAnalysis.py classifyFile -i <inputFilePath> --model <svm, knn, extratrees, gradientboosting or randomforest> --classifier <pathToClassifierModeL>"
python ../audioAnalysis.py classifyFile -i $file --model svm --classifier ../data/svmSM
python ../audioAnalysis.py classifyFile -i $file --model knn --classifier ../data/knnSM
python ../audioAnalysis.py classifyFile -i $file --model extratrees --classifier ../data/etSM
python ../audioAnalysis.py classifyFile -i $file --model gradientboosting --classifier ../data/gbSM
python ../audioAnalysis.py classifyFile -i $file --model randomforest --classifier ../data/rfSM
python ../audioAnalysis.py classifyFile -i $file --model svm --classifier ../data/svmMovies8classes
python ../audioAnalysis.py classifyFile -i $file --model knn --classifier ../data/knnMovies8classes
python ../audioAnalysis.py classifyFile -i $file --model knn --classifier ../data/knnSpeakerAll

echo "single file regression"
python ../audioAnalysis.py regressionFile -i $file --model svm --regression ../data/svmSpeechEmotion

echo "file segmentation into trained regression classes"
python ../audioAnalysis.py segmentClassifyFile -i $file --model svm --modelName ../data/svmSM

echo "generate audio thumbnail"
echo "python ../audioAnalysis.py thumbnail -i <wavFileName> --size <thumbnailDuration>"
python ../audioAnalysis.py thumbnail -i $file

echo "TODO hmm and default classifiers";
done
