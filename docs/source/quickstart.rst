.. _quickstart:

Quick Start
==================================
You can try out m6anet by simply running it over our sample dataset that comes with this repo. Simply run::

    tar -xvf demo_data.tar.gz .

After extraction, you will find::

    demo_data
        |-- eventalign.txt
        |-- summary.txt

Here eventalign.txt and summary.txt are the segmented nanopore raw signal files produced by `Nanopolish Eventalign <https://nanopolish.readthedocs.io/en/latest/quickstart_eventalign.html>`_ functionality.

Firstly, we need to preprocess the segmented raw signal file in the form of nanopolish eventalign file using 'm6anet-dataprep'::

    m6anet-dataprep --eventalign demo_data/eventalign.txt \
                    --summary demo_data/summary.txt \
                    --out_dir demo_data --n_processes 4

The output files are stored in ``demo_data``:

* ``eventalign.hdf5``: Merged segments from ``nanopolish eventalign``, stored with the hierarchical keys ``<TRANSCRIPT_ID>/<READ_ID>/events``
* ``eventalign.log``: Log file containing read ids of successfully merged reads
* ``inference``: A folder storing individual potential m6a sites for inference
* ``prepare_for_inference.log``: A log file containing successfully preprocessed transcripts from eventalign.hdf5 for inference

Now we can run m6anet over our data using m6anet-inference::

    m6anet-inference -i demo_data -o demo_data -m m6anet/model/ -n 4

The output files `demo_data/m6Anet_predictions.csv.gz` contains the probability of modification at each individual position for each transcript.
