<p align="center"><i>
  Kaldi-ASR  Basic Project <br/>
  T-718-ATSR - Automatic Speech Recognition, TD-MSc, 2019-1 <br/>
  Reykjavik University - School of Computer Science, Menntavegur 1, IS-101 Reykjavik, Iceland
</i></p>

## 1 Introduction
A modified version of the [Kaldi-ASR yesno](https://github.com/kaldi-asr/kaldi/tree/master/egs/yesno) example project adjusted for Icelandic and for multiple speakers.


## 2 The Dataset
Custom yes/no dataset for Icelandic, made from recordings from students and the staff members from the course.

Summary:
* 10 different speakers
* 120 sentences/utterances  
 * Speaker 0: 10 utterances
 * Speaker 1: 7 utterances
 * Speaker 2: 8 utterances
 * Speaker 3: 10 utterances
 * Speaker 4: 10 utterances
 * Speaker 5: 42 utterances
 * Speaker 6: 0 utterances
 * Speaker 7: 10 utterances
 * Speaker 8: 8 utterances
 * Speaker 9: 10 utterances
 * Speaker 10: 5 utterances
* Each utterance consist of 8 words.
* The vocabulary for the words:
 * Já (yes)
 * Nei (no

## 3 The Adjustments

### run.sh

Edit following part in the file:

```bash
if [ ! -d waves_yesno ]; then
  wget waves_yesno.tar.gz || exit 1;
  tar -xvzf waves_yesno.tar.gz || exit 1;
fi
```

We have our the dataset locally, so we dont have to fetch it from remote file host.

Remember to re-compile the script.
```bash
$ chmod 777 s5/run.sh
```

### local/create_yesno_txt.pl

Add following code to the while loop:

```perl
    $trans =~ s/SA\d\d-//;
```

This will remove the speaker Id from our filename when creating our data/{X}/text files.


### local/create_yesno_waves_test_train.pl

Specify which speakers you want to have in the test set.

```perl
while ($l = <FL>)
{
	chomp($l);
	if (index($l, "SA01") == -1  && index($l, "SA10") == -1)
	{
		print TRAINLIST "$l\n";
	}
	else
	{
		print TESTLIST "$l\n";
	}
}
```

### local/prepare_data.sh 

Edit the code so each utterece has it corresponding speaker id istead of global.

```bash
  cat data/$x/text | awk '{printf("%s %s\n", $1, substr($1, 0, 5));}' > data/$x/utt2spk
```

Remember to re-compile the script.
```bash
$ chmod 777 s5/local/prepare_data.sh 
```

### conf/mfcc.conf
Set the correct sample-frequency configuration based on the waves_yesno .wav file format.

```bash
--sample-frequency=16000 #  waves_yesno is sampled at 16kHz
```

### Done

Now run run.sh file

```bash
$ bash run.sh
```
Expected Output:

```bash

```

## 4 Authors
* [Egill Anton Hlöðversson](https://github.com/egillanton) - MSc. Language Technology Student

## 5 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 6 References
* [Kaldi-ASR](http://kaldi-asr.org/)

<p align="center">
🌟 PLEASE STAR THIS REPO IF YOU FOUND SOMETHING INTERESTING 🌟
</p>