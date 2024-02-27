# ndx-aind-metadata Extension for NWB

Extension for Allen Institute Neural Dynamics

## Installation


## Usage

```python
import datetime

from ndx_aind_metadata import (
    AindSubject,
    AindSession,
    AindRig,
    AindDataDescription,
    AindProcedures,
    AindProcessing,
)
from pynwb import NWBFile, NWBHDF5IO
from pathlib import Path

nwbfile = NWBFile(
    session_description="test",
    identifier="test",
    session_start_time=datetime.datetime.now(),
)

data_dir = Path("/Users/bendichter/Downloads")


with open(data_dir / "subject.json", "r") as f:
    subject_json_text = f.read()

nwbfile.subject = AindSubject(aind_schema_json=subject_json_text)


for cls, filepath in (
        (AindDataDescription, data_dir / "data_description.json"),
        (AindProcedures, data_dir / "procedures.json"),
        (AindProcessing, data_dir / "processing.json"),
        (AindRig, data_dir / "ephys_rig.json"),
        (AindSession, data_dir / "ephys_session.json"),
):
    with open(filepath, "r") as f:
        nwbfile.add_lab_meta_data(cls(aind_schema_json=f.read()))

with NWBHDF5IO("test_example.nwb", "w") as io:
    io.write(nwbfile)
```

---
This extension was created using [ndx-template](https://github.com/nwb-extensions/ndx-template).
