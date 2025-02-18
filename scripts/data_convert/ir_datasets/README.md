## Configurable pipelines for IR-datasets

We support (somewhat experimentally) simple conversion pipelines for [ir-datasets](https://ir-datasets.com/).
They are processed using the script [configurable_convert.py](configurable_convert.py),
which is guided by a JSON configuration file. 
The output of this file is one or more JSONL files, 
which can be processed using standard FlexNeuART tools.

There are examples
for [Cranfield](sample_configs/cranfield.json) and [ClueWeb12B](sample_configs/clueweb12-b13.json).
Although some datasets will be automatically downloaded by `ir-datasets`, 
many datasets are licensed and need to be installed **manually**.

The JSON configuration file contains an array of descriptions, each of which is supposed
to be applicable to a specific dataset part: a dataset split. 
The dataset split is identified by the IR-dataset name (attribute `dataset_name`).
It has two crucial parameters:
1. a destination sub-folder in the output catalog (attribute `part_name`);
2. a flag specifyng whether the split contains queries (rather than documents).
3. a list of input attributes (except query and document IDs), which we also call fields (parameter `src_attributes`).

Each pipeline stage accepts a number of attributes/fields and converts/processes them. 
The output of the stage is a set of fields, which can have different values and names.
Each stage uses one ore more processing components: There is an array of component descriptions.
Components are applied to a set of input attributes one by one and they produce
output attributes for the next pipeline stage.

In the simplest case,
no data processing is happening and the attribute/field is renamed.
Another simple operation is concatenation of fields.

Crucially, there are no "fall through" attributes, which pass a pipeline stage without explicitly defined operation. 
If we need to pass some document attribute "as is", we need to use either a `copy` or `rename`
processing component.

