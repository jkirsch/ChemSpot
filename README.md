# ChemSpot

ChemSpot is a named entity recognition tool for identifying mentions of chemicals in natural language texts, including trivial names, drugs, abbreviations, molecular formulas and IUPAC entities. Since the different classes of relevant entities have rather different naming characteristics, ChemSpot uses a hybrid approach combining a Conditional Random Field with a dictionary. It achieves an F1 measure of 72.1% on the SCAI corpus.

ChemSpot is released under the Common Public License 1.0 (see LICENSE).

The warning message "Couldn't open cc.mallet.util.MalletLogger resources/logging.properties file." can be ignored.

## Running ChemSpot:
- Extract chemspot.tar.gz into a directory
```
tar -xvf chemspot.tar.gz
```
- To tag a sample text file, run
```
java -jar -Xmx9G chemspot.jar -m crf_model.bin -s sentence_model.bin.gz -d dict.zip -i ids.zip -t sample.txt -Io predict.iob
```

## Parameters
  mandatory arguments:
    -m path to a CRF model file
    -s path to a OpenNLP sentence model file

  optional arguments:
    -d path to a zipped set of brics dictionary automata
    -i path to a zipped tab-separated text file representing a map of terms to ids
    -e if this parameter is set, the performance of ChemSpot on a IOB gold-standard corpus (cf. -c) or CRAFT corpus is evaluated
    -T number of threads to create when processing a document collection

  input control:
    -c path to a directory containing corpora in IOB format that should be tagged
    -C path to a directory containing CRAFT corpus files in UIMA XMI format that should be tagged
    -G path to a directory containing gzipped text files that should be tagged
    -t path to a text file that should be tagged

  output control:
    -o path to output file
    -I set if output should be converted to IOB format


## Using ChemSpot in your Code
```java
        ChemSpot tagger = new ChemSpot("./crf_model.bin", "./sentence_model.bin.gz", "./dict.bin", "./ids.zip");
        String text = "The abilities of LHRH and a potent LHRH agonist "
          + "([D-Ser-(But),6, des-Gly-NH210]LHRH ethylamide) inhibit FSH "
          + "responses by rat granulosa cells and Sertoli cells in vitro have been compared.";
        for (Mention mention : tagger.tag(text)) {
            System.out.printf("%d\t%d\t%s\t%s\t%s\n", 
                    mention.getStart(), mention.getEnd(), mention.getText(), 
                    mention.getId(), mention.getSource());
        }
```

## Reproducing our Results
- Download the SCAI corpus (chemicals-test-corpus-27-04-2009-v3.iob.gz) and put it in the same directory
- To reproduce our results, run
```
java -jar -Xmx9G chemspot.jar -m crf_model.bin -s sentence_model.bin.gz -d dict.zip -i ids.zip -c chemicals-test-corpus-27-04-2009-v3.iob.gz -o predict.iob -e
```
- Evaluate ChemSpot using the Scala script
```
scala eval.scala chemicals-test-corpus-27-04-2009-v3.iob.gz predict.iob
```

## Acknowledgements
We would like to thank Daniel Lowe and Philippe Thomas for many valuable suggestions. 
