# Sparrow Parse

## Description

This module implements Sparrow Parse [library](https://pypi.org/project/sparrow-parse/) with helpful methods for data pre-processing, parsing and extracting information.

## Install

```
pip install sparrow-parse
```

## Pre-processing

### Unstructured

```
from sparrow_parse.extractor.unstructured_processor import UnstructuredProcessor

processor = UnstructuredProcessor()

content, table_content = processor.extract_data(
        file_path,  # file to process
        strategy,  # data processing strategy supported by unstructured
        model_name,  # model supported by unstructured
        options,  # table extraction into HTML format
        local,  # True if running from CLI, or False if running from FastAPI
        debug)  # Debug
```

Example:

*file_path* - `/Users/andrejb/infra/shared/katana-git/sparrow/sparrow-ml/llm/data/invoice_1.pdf`

*strategy* - `hi_res`

*model_name* - `yolox`

*options* - `['tables', 'unstructured']`

*local* - `True`

*debug* - `True`

### Markdown

```
from sparrow_parse.extractor.markdown_processor import MarkdownProcessor

processor = MarkdownProcessor()

content, table_content = processor.extract_data(
        file_path,  # file to process
        options,  # table extraction into HTML format
        local,  # True if running from CLI, or False if running from FastAPI
        debug)  # Debug
```

Example:

*file_path* - `/Users/andrejb/infra/shared/katana-git/sparrow/sparrow-ml/llm/data/invoice_1.pdf`

*options* - `['tables', 'markdown']`

*local* - `True`

*debug* - `True`

## Parsing and extraction

### HTML extractor

```
from sparrow_parse.extractor.html_extractor import HTMLExtractor

extractor = HTMLExtractor()

answer, targets_unprocessed = extractor.read_data(
        target_columns,  # list of table columns data to fetch
        data, # list of HTML tables
        column_keywords,  # list of valid column names, can be empty. Useful to filter junk content
        group_by_rows,  # JSON result grouping
        update_targets,  # Set to true, if page contains multiple tables with the same columns
        local,  # True if running from CLI, or False if running from FastAPI
        debug)  # Debug

```

Example:

*target_columns* - `['description', 'qty', 'net_price', 'net_worth', 'vat', 'gross_worth']`

*data* - `list of HTML tables`

*column_keywords* - `None`

*group_by_rows* - `True`

*update_targets* - `True`

*local* - `True`

*debug* - `True`

### Sparrow Parse VL (vision-language model) extractor

```
extractor = VLLMExtractor()

# export HF_TOKEN="hf_"
config = {
    "method": "huggingface",  # Could be 'huggingface' or 'local_gpu'
    "hf_space": "katanaml/sparrow-qwen2-vl-7b",
    "hf_token": os.getenv('HF_TOKEN'),
    # Additional fields for local GPU inference
    # "device": "cuda", "model_path": "model.pth"
}

# Use the factory to get the correct instance
factory = InferenceFactory(config)
model_inference_instance = factory.get_inference_instance()

input_data = [
    {
        "image": "/Users/andrejb/Documents/work/epik/bankstatement/bonds_table.png",
        "text_input": "retrieve financial instruments data. return response in JSON format"
    }
]

# Now you can run inference without knowing which implementation is used
result = extractor.run_inference(model_inference_instance, input_data, generic_query=False, debug=True)
print("Inference Result:", result)
```

## PDF optimization

```
from sparrow_parse.extractor.pdf_optimizer import PDFOptimizer

pdf_optimizer = PDFOptimizer()

num_pages, output_files, temp_dir = pdf_optimizer.split_pdf_to_pages(file_path,
                                                                     output_directory,
                                                                     convert_to_images)

```

Example:

*file_path* - `/Users/andrejb/infra/shared/katana-git/sparrow/sparrow-ml/llm/data/invoice_1.pdf`

*output_directory* - set to not `None`, for debug purposes only

*convert_to_images* - default `False`, to split into PDF files

## Library build

Create Python virtual environment

```
python -m venv .env_sparrow_parse
```

Install Python libraries

```
pip install -r requirements.txt
```

Build package

```
pip install setuptools wheel
python setup.py sdist bdist_wheel
```

Upload to PyPI

```
pip install twine
twine upload dist/*
```

## Commercial usage

Sparrow is available under the GPL 3.0 license, promoting freedom to use, modify, and distribute the software while ensuring any modifications remain open source under the same license. This aligns with our commitment to supporting the open-source community and fostering collaboration.

Additionally, we recognize the diverse needs of organizations, including small to medium-sized enterprises (SMEs). Therefore, Sparrow is also offered for free commercial use to organizations with gross revenue below $5 million USD in the past 12 months, enabling them to leverage Sparrow without the financial burden often associated with high-quality software solutions.

For businesses that exceed this revenue threshold or require usage terms not accommodated by the GPL 3.0 license—such as integrating Sparrow into proprietary software without the obligation to disclose source code modifications—we offer dual licensing options. Dual licensing allows Sparrow to be used under a separate proprietary license, offering greater flexibility for commercial applications and proprietary integrations. This model supports both the project's sustainability and the business's needs for confidentiality and customization.

If your organization is seeking to utilize Sparrow under a proprietary license, or if you are interested in custom workflows, consulting services, or dedicated support and maintenance options, please contact us at abaranovskis@redsamuraiconsulting.com. We're here to provide tailored solutions that meet your unique requirements, ensuring you can maximize the benefits of Sparrow for your projects and workflows.

## Author

[Katana ML](https://katanaml.io), [Andrej Baranovskij](https://github.com/abaranovskis-redsamurai)

## License

Licensed under the GPL 3.0. Copyright 2020-2024 Katana ML, Andrej Baranovskij. [Copy of the license](https://github.com/katanaml/sparrow/blob/main/LICENSE).
