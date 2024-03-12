# Southern Cross AI Training Data Creator
This repository is intended to preserve the code used to collect the training data for the Southern Cross LLM. It is currently a work-in-progress.

## Sources
Provided below is a breakdown of potential open data sources by their size, quality, diversity and ease of scraping.
| Source                       | Size  | Quality | Diversity | Ease of Access | Priority       |
| ---------------------------- | ----- | ------- | --------- | -------------- | -------------- |
| Open Australian Legal Corpus | Large | High    | Low       | Extremely easy | Extremely high |
| Parliament of Australia      | Large | High    | Medium    | Very difficult | High           |
| data.gov.au                  | Large | Medium  | High      | Very difficult | Medium         |

Possible sizes are extremely large (≥1M documents), very large (≥500k), large (≥100k), medium (≥50k), small (≥10k), very small (≥5k) and extremely small (<5k).

Possible qualities, diversities and priorities are extremely high, very high, medium, low, very low and extremely low.

Possible eases of access are extremely easy (requires no effort to transform into the desired format), very easy (little effort required), somewhat easy (some effort required), difficult (requires substantial effort required), very difficult (large amount of effort required) and extremely difficult (extreme amount of effort required).

Feel free to update this table with other suggested open data sources.

## Format
The final training dataset will be a sequence of `text`s. For the purposes of the dataset, a `text` refers to a standalone piece of text, whether it be in the form of a blog post, HTML file, scanned PDF or any other common format text is capable of taking.

A `text` will conform to the following schema, expressed, for the sake of convenience, as a [msgspec struct](https://jcristharif.com/msgspec/structs.html):
```python
import msgspec

class Text(msgspec.Struct, array_like=True):
    id: str
    """A unique identifier for the text incorporating the name of the data source.

    The identifier should begin with the name of the direct source of the text followed by a colon and then a unique
    identifier within that data source.

    The identifier should be, in order of precedence, the most authoritative, stable and semantic identifier available.
    As an absolute last resort, the identifier may use the XXH3 64-bit hexdisgest hash of the text itself."""
    
    text: str
    """The text itself."""

    when: float
    """The Unix timestamp of when the text was scraped."""
    
    source: str
    """A canonical name for the direct source of the text."""
    
    uri: str = None
    """The most authoritative uri (ideally, a url) possible for the text, if available."""
```
