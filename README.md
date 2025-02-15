# Assignment05

#### section 3 UID-decoding
Modification in translate_beam.py and ./seq2seq/beam.py with an addtional eval_p function, change the score where each token adds to BeamSearchNode class.

Change paths to your own data files
```
python /content/drive/MyDrive/ATML/translate_beam.py \
    --data /content/drive/MyDrive/ATML/data/en-fr \
    --batch-size 16\
    --beam-size 5\
    --dicts /content/drive/MyDrive/ATML/data/en-fr \
    --checkpoint-path /content/drive/MyDrive/ATML/Assignment03/checkpoints_lexical/checkpoint_best.pt \
    --output /content/drive/MyDrive/ATML/Assignment05/model_translations_diverse_2.txt \
    --cuda True\
    --regulation True
```

#### section 4 Investigating the Diversity of Beam Search
Modification in translate_beam.py and ./seq2seq/beam.py, change the score add to the search.nodes.queue

Change paths to your own data files
```
python /content/drive/MyDrive/ATML/translate_beam.py \
    --data /content/drive/MyDrive/ATML/data/en-fr \
    --batch-size 16\
    --beam-size 5\
    --dicts /content/drive/MyDrive/ATML/data/en-fr \
    --checkpoint-path /content/drive/MyDrive/ATML/Assignment03/checkpoints_lexical/checkpoint_best.pt \
    --output /content/drive/MyDrive/ATML/Assignment05/model_translations_diverse_2.txt \
    --cuda True\
    --diversity True\
    --n-best 3\
    --lamda 2
```

(Some testing results are stored in ./assginment/05)



# Assignment03
Try two strategies to improve the performance of low-resource NMT model.

(All the testing results are stored in ./assginment/03)

#### First strategy: 

Tuning Hyper-parameters like batch size, different layers of LSTM model, learning rate.

#### Second strategy: 

Implement a lexical model to improve lexical choice.

(Modification for coding in ./seq2seq/models/lstm.py Class LSTMDecoder)
# Training a model

#### Training a baseline model

```
python train.py \
    --data path/to/prepared/data \
    --source-lang fr \
    --target-lang en \
    --save-dir path/to/model/checkpoints \
```
#### Tuning a hyper-parameter on the baseline model

Change the batch size:

```
python train.py \
    --data path/to/prepared/data \
    --source-lang fr \
    --target-lang en \
    --save-dir path/to/model/checkpoints \
    --batch-size 16
```
Change the model depth:

```
python train.py \
    --encoder-num-layers 3\
    --decoder-num-layers 3\
    --data path/to/prepared/data \
    --source-lang fr \
    --target-lang en \
    --save-dir path/to/model/checkpoints \
```

#### Training with lexical model

```
python train.py \
    --data path/to/prepared/data \
    --source-lang fr \
    --target-lang en \
    --save-dir path/to/model/checkpoints \
    --decoder-use-lexical-model True
```

# Evaluating a trained model

Run inference on test set
```
python translate.py \
    --data path/to/prepared/data \
    --dicts path/to/prepared/data \
    --checkpoint-path path/to/model/checkpoint/file/for/loading \
    --output path/to/output/file/model/translations
```

Postprocess model translations
```
bash scripts/postprocess.sh path/to/output/file/model/translations path/to/postprocessed/model/translations/file en
```

Score with SacreBLEU
```
cat path/to/postprocessed/model/translations/file | sacrebleu path/to/raw/target/test/file
```
