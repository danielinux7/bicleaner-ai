Unreleased:
* Update to Hardrules 2.0
    * Rules can be parametrized with `--rules_config config.yaml`
    * Some rules have been refactored with better names.
    * `--run_all_rules` mode to run each rule instead of stoppping at first discard
    * Language identification with [FastSpell](https://github.com/mbanon/fastspell)
* Huge memory improvements during training.
* Hide Tensorflow and Transformers logging messages in executable scripts.
* Update HF Transformers, no longer needed single GPU for prediction.
* Avoid generating empty sentences in omit noise.
* Restore capital letters at the beggining of the sentennce in frequency noise.
* Fix loading lite models in other other Python versions than 3.8.
* Other minor fixes.

Bicleaner AI 1.0.1:
* Update hardrules to 1.2: adds score only mode.

Bicleaner AI 1.0:
* Bicleaner train changes:
  * Separate most of the training logic in the BaseModel class.
  * Re-factor synthetic noise build function.
  * Parallelize synthetic noise generation.
  * Add fuzzy matching noise and neighbour noise.
  * Add Decomposable Attention model.
  * Add Transkformer-like model.
  * Add XLMRoberta model.
* Bicleaner classify changes:
  * Change old classifier by new neural models.
  * Move hardrules into a separate package.

Bicleaner 0.15:
* Bicleaner train changes:
  * Qmax bug fixing.
  * Classifier training uses the number of processes given by argument.
* Bicleaner classify changes:
  * Refactored classifier scripts: code cleaning and remove lot of duplicated code.
  * Buffered tokenization: improve speed of external tokenization tokenizing blocks of lines instead of line by line.

Bicleaner 0.14: 
* Bicleaner hardrules changes:
  * New rule: filter out sentences containing gluedWordsLikeThis.
  * Rule change: Relaxed c_different_language rule for similar languages.
  * New rule: filter out porn sentences using FastText classifier.
  * Parameters changed: `-s/--source_lang` and `-t/--target_lang` are no longer mandatory (if a metadata .yaml file is provided)
* Bicleaner train changes:
  * Default classifier is now `extra_trees`
  * New parameters: `-f`  and `-F`, source and target word frequency dictionaries.
  * New qmax features:
    * `qmax_nosmooth_nolimit_freq`: removes OOV smoothing, word limits and weights each target word with its monolingual probability using the word frequency dictionary.
    * `qmax_nosmooth_nolimit_cummulated_prob_zipf_freq`: uses accumulated probability instead of maximum and splits the score into quartiles based on word frequencies.
  * Added more bilingual dictionary coverage features, splitting them into quartiles based on monolingual word frequencies.
  * Added new noise function that synthesizes negative samples cutting sentences and replacing words (this is not used by default, needs more testing).
  * Changed classifier training behavior and use grid search.
  * Removed `bicleaner_train_lite.py`
  * Removed parameters: `-g` (`--good_examples`) and `-w` (`--wrong_examples`):
    * Now, training automatically uses one half of the input file for good examples and the other half to synthesize wrong examples.
    * Of this partitions, 90% will be used for training and the remaining 10% for testing.
  * New parameter: `--relative_paths` allows to save model files paths relative instead of absolute  (useful for training distributable models)
  * Changed logging info messages, now more informative.
* Other
   * Now using [sacremoses](https://github.com/alvations/sacremoses) instead of [mosestokenizer](https://github.com/luismsgomes/mosestokenizer)
   * New script:  `./utils/download-pack.sh` allows to download language packs for a given language pair.


Bicleaner 0.13:
* Bicleaner hardrules changes:
  * Rule change: Relaxed c_minimal_length to accept 3-word sentences	
  * New feature: LM filtering (moved from Bicleaner Classify)
  * New parameter: `--disable_lm_filter`, `--metadata` and `--lm_threshold`, to support LM filtering
* Bicleaner training changes: 
  * New parameter: Features relying on language identification can be disabled with flag `--disable_lang_ident` (this will be outputed in the .yaml file and used by Bicleaner clasifier)
  * New feature: Debug mode now gives information on random forest feature importances
  * Parameter change: --noisy_examples_file_sl and --noisy_examples_file_tl are now optional
  * Parameter change: input now must be more than 10K sentences long
  * Removed INFO messages when processes starting/ending (except when debugging)
* Bicleaner classifier changes:
  * `--disable_lang_ident` flag is now read from the .yaml file
  * Removed feature: LM filtering (moved to Bicleaner Hardrules)
  * New parameter: `--disable_lm_filter`
  * Removed parameters: `--keep_lm_result`,  `--threshold`
* Other:
  * Updated requirements
  
  
  
Bicleaner 0.12:
* Bicleaner hardrules changes:
  * New rule: c_identical_wo_punct to reject sentences only different in punctuation (and it's case insensitive)
  * New rule:  Sentences containing "Re:" are rejected
  * Rule change: c_minimal_length now rejects sentences with both sides <= 3 words (instead of only one)
  * Rule change: c_identical and c_identical_wo_digits now is case insensitive
  * Rule change: Breadcrumbs rule now split into c_no_breadcrumbs1 and c_no_breadcrumbs2
  * Rule change: Breadcrumbs2 now includes character "·" in the rejected characters
  * Rule change: c_length now compares byte length ratio (will avoid rejecting valid sentences due to length ratio when comparing languages with different alphabets)
  * Changed behaviour for `--annotated_output` argument in hardrules. See README.md for more information.
  * New parameter: `--disable_lang_ident` flag to avoid applying rules that need to identify the language
* Bicleaner classify changes:  
  * Now using only 3 decimal places for Bicleaner score and LM score
  * Removed INFO messages when processes starting/ending (except when debugging)
  * New parameter: '--disable_hardrules' flag to avoid applying hardrules
  * New parameter: '--disable_lang_ident' flag to avoid applying rules that need to identify the language
  * New parameter: '--score_only' flag to output only Bicleaner scores (proposed by [@kirefu](https://github.com/kirefu))
* Bicleaner features changes:
  * Fixed bug when probability in prob_dict is 0 (issue [#19](https://github.com/bitextor/bicleaner/issues/19))
* Other:
  * Fixed sklearn version to 0.19.1
  * Added utilities for training: `shuffle.py` and `dict_pruner.py`
  * Updated instalation guides in readme
