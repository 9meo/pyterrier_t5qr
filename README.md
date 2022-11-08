# PyTerrier_monoQA

This is the [PyTerrier](https://github.com/terrier-org/pyterrier) plugin for the [T5QR]() T5 query rewriting approaches

## Installation

This repostory can be installed using Pip.

    pip install --upgrade git+https://github.com/9meo/pyterrier_t5qr.git
    
## Building T5QR pipelines

```python
import pyterrier as pt
from pyterrier_t5qr import T5QR
t5qr = T5QR() # loads castorini/t5-base-canard by default


dataset = pt.get_dataset("irds:vaswani")
bm25 = pt.BatchRetrieve(pt.get_dataset("vaswani").get_index(), wmodel="BM25")
t5qr_pipeline = bm25 >> pt.text.get_text(dataset, "text") >> t5qr    
```

Note that both approaches require the document text to be included in the dataframe (see [pt.text.get_text](https://pyterrier.readthedocs.io/en/latest/text.html#pyterrier.text.get_text)).

monoQA has the following options:
 - `model` (default: `'9meo/monoQA'`). HGF model name. Defaults to a version trained on OR-QuAC.
 - `tok_model` (default: `'9meo/monoQA'`). HGF tokenizer name.
 - `batch_size` (default: `4`). How many documents to process at the same time.
 - `text_field` (default: `text`). The dataframe attribute in which the document text is stored.
 - `verbose` (default: `True`). Show progress bar.
 
 
## Examples

Checkout out the notebooks, even on Colab: 
    - Vaswani [[Github]([https://github.com/9meo/pyterrier_t5qr/blob/main/pyterrier_t5qr_vaswani.ipynb)] [[Colab](https://colab.research.google.com/github/9meo/pyterrier_t5qr/blob/main/pyterrier_t5qr_vaswani.ipynb)]
    
## Implementation Details

We use a PyTerrier transformer to rewrite query using a T5 model.

Sequences longer than the model's maximum of 512 tokens are silently truncated. Consider splitting long texts
into passages and aggregating the results ([examples](https://pyterrier.readthedocs.io/en/latest/text.html#working-with-passages-rather-than-documents)).

## References

  - <a id="Macdonald20"/>Craig Macdonald, Nicola Tonellotto. Declarative Experimentation inInformation Retrieval using PyTerrier. Craig Macdonald and Nicola Tonellotto. In Proceedings of ICTIR 2020. https://arxiv.org/abs/2007.14271

## Credits

- Sarawoot Kongyoung, University of Glasgow