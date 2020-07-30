# 1. Copy contents of https://github.com/stanfordnmbl/kneenet-docker repository to kneenet-directory

git clone https://github.com/stanfordnmbl/kneenet-docker.git

# 2. Enter the kneenet-docker directory

cd kneenet-docker

# 3. Download per-built docker and model weights

wget https://kidzinski.s3-eu-west-1.amazonaws.com/dockers/kneenet-latest.tar.gz
wget https://kidzinski.s3-eu-west-1.amazonaws.com/models/KneeNet/KneeNet.0

# 4. Load pre-built docker 'kneenet-latest.tar.gz'

docker load < kneenet-latest.tar.gz

# 5. Move model weights 'KneeNet.0' to models directory

mkdir models
mv KneeNet.0 models/

# 6. Process images from 'input' directory

docker run \
    -v ${PWD}/input:/workspace/input \
    -v ${PWD}/output:/workspace/output \
    -v ${PWD}/scripts:/workspace/scripts \
    -v ${PWD}/models:/workspace/models \
    -it kidzik/kneenet:latest \
    python scripts/predict.py && printf "\n-- RESULTS (in output/prediction.csv) --\n" && cat output/predictions.csv | column -t -s,
