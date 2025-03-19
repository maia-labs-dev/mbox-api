# API Documentation  
Click here to view the Swagger documentation:
[Swagger UI](https://editor.swagger.io/?url=https://raw.githubusercontent.com/maia-labs-dev/mbox-api/main/api/maia-lab-swagger-new.json)

## Models

```
# Enumerations
class ExaminationStateType(str, Enum):
    planned = 'planned'
    current = 'current'
    next_planned = 'next_planned'
    performed = 'performed'
    verified = 'verified'
    processing = 'processing'
    stopping = 'stopping'


class SourceType(str, Enum):
    manual = 'manual'
    automatic = 'automatic'


class ReportStatus(Enum):
    FINAL = "final"
    REGISTERED = "registered"
    PARTIAL = "partial"
    PRELIMINARY = "preliminary"
    MODIFIED = "modified"
    AMENDED = "amended"
    CORRECTED = "corrected"
    APPENDED = "appended"
    CANCELLED = "cancelled"
    ENTERED_IN_ERROR = "entered-in-error"
    UNKNOWN = "unknown"


class TypeEnum(str, Enum):
    string = "string"
    float = "float"
    int = "int"
    bool = "bool"


# Models
class StringValueTypePair(BaseModel):
    """Pair representing a string value with its type and source."""
    value: Optional[str] = Field(None, description="The actual string value.")
    type: Optional[TypeEnum] = Field(None, description="The data type of the value (e.g., string, float).")
    source: Optional[SourceType] = Field(None, description="Source of the value (manual or automatic).")


class BooleanValueTypePair(BaseModel):
    """Pair representing a boolean value with its type and source."""
    value: Optional[bool] = Field(None, description="The actual boolean value (true/false).")
    type: Optional[TypeEnum] = Field(None, description="The data type of the value (e.g., bool).")
    source: Optional[SourceType] = Field(None, description="Source of the value (manual or automatic).")


class FloatValueTypePair(BaseModel):
    """Pair representing a float value with its type and source."""
    value: Optional[float] = Field(None, description="The actual float value.")
    type: Optional[TypeEnum] = Field(None, description="The data type of the value (e.g., float).")
    source: Optional[SourceType] = Field(None, description="Source of the value (manual or automatic).")


class Snapshot(BaseModel):
    """Represents a snapshot taken during an examination."""
    id: Optional[str] = Field(None, description="Unique identifier of the snapshot.")
    timestamp: Optional[str] = Field(None, description="Absolute timestamp of when the snapshot was taken (ISO format).")
    timestamp_relative: Optional[float] = Field(None, description="Relative time in seconds from the start of the examination.")
    image_id: Optional[str] = Field(None, description="Identifier of the associated image file.")


class Sample(BaseModel):
    """Represents a sample taken during an examination."""
    value: Optional[bool] = Field(None, description="Indicates if a sample was taken (true/false).")
    id: Optional[str] = Field(None, description="Unique identifier of the sample.")
    type: Optional[TypeEnum] = Field(None, description="Data type of the sample value (e.g., string, float).")
    source: Optional[SourceType] = Field(None, description="Source of the sample data (manual or automatic).")


class Statistics(BaseModel):
    """Statistical data collected during an examination."""
    duration: FloatValueTypePair = Field(..., description="Duration of the examination in seconds.")
    extraction_time: StringValueTypePair = Field(..., description="Time taken for extraction process.")
    bbps: StringValueTypePair = Field(..., alias="BBPS", description="Boston Bowel Preparation Scale score.")
    visibility: StringValueTypePair = Field(..., description="Visibility conditions during the examination.")
    ileum_reached: BooleanValueTypePair = Field(..., description="Indicates if the ileum was reached.")
    cecum_reached: BooleanValueTypePair = Field(..., description="Indicates if the cecum was reached.")
    tools_used: List[StringValueTypePair] = Field(..., description="List of tools used during the examination.")

    class Config:
        populate_by_name = True  # Allow using "BBPS" as alias


class Finding(BaseModel):
    """Represents a medical finding observed during the examination."""
    id: Optional[str] = Field(None, description="Unique identifier of the finding.")
    position: StringValueTypePair = Field(..., description="Anatomical position of the finding.")
    size: Optional[StringValueTypePair] = Field(None, description="Size of the finding (e.g., in mm).")
    classification_paris: Optional[StringValueTypePair] = Field(None, description="Paris classification of the finding.")
    tool: Optional[StringValueTypePair] = Field(None, description="Tool used to identify or treat the finding.")
    raw_transcription: Optional[str] = Field(None, description="Raw transcription text from the finding.")
    sample_taken: Optional[Sample] = Field(None, description="Details of any sample taken from the finding.")
    snapshot_ids: List[Snapshot] = Field(..., description="List of snapshots associated with this finding.")


class Note(BaseModel):
    """A note recorded during the examination."""
    id: Optional[str] = Field(None, description="Unique identifier of the note.")
    position: StringValueTypePair = Field(..., description="Anatomical position related to the note.")
    raw_transcription: Optional[str] = Field(None, description="Raw transcription text of the note.")
    snapshot_ids: List[Snapshot] = Field(..., description="List of snapshots associated with this note.")


class Patient(BaseModel):
    """Information about the patient undergoing the examination."""
    uuid: Optional[str] = Field(None, description="Universally unique identifier of the patient.")
    mrn: Optional[str] = Field(None, description="Medical Record Number of the patient.")
    id: Optional[str] = Field(None, description="Local patient identifier.")
    given_name: Optional[str] = Field(None, description="Patient's first name.")
    surname: Optional[str] = Field(None, description="Patient's last name.")
    assigning_authority_OID: Optional[str] = Field(None, description="OID of the assigning authority.")
    birth_date: Optional[str] = Field(None, description="Patient's date of birth (ISO format).")
    age: Optional[int] = Field(None, description="Patient's age in years.")


class Practitioner(BaseModel):
    """Information about the practitioner performing the examination."""
    id: Optional[str] = Field(None, description="Unique identifier of the practitioner.")
    given_name: Optional[str] = Field(None, description="Practitioner's first name.")
    surname: Optional[str] = Field(None, description="Practitioner's last name.")


class Nurse(BaseModel):
    """Information about the nurse assisting in the examination."""
    id: Optional[str] = Field(None, description="Unique identifier of the nurse.")
    given_name: Optional[str] = Field(None, description="Nurse's first name.")
    surname: Optional[str] = Field(None, description="Nurse's last name.")


class ReportText(BaseModel):
    """Text content of the examination report."""
    text: Optional[str] = Field(None, description="Full text of the report.")
    timestamp: Optional[str] = Field(None, description="Timestamp of when the report text was created (ISO format).")


class Report(BaseModel):
    """Comprehensive report of a medical examination."""
    id: str = Field(..., description="Unique identifier of the report.")
    time_start: str = Field(..., description="Start time of the examination (ISO format).")
    time_end: str = Field(..., description="End time of the examination (ISO format).")
    practitioner: Practitioner = Field(..., description="Details of the practitioner performing the examination.")
    nurse: Nurse = Field(..., description="Details of the nurse assisting in the examination.")
    patient: Patient = Field(..., description="Details of the patient undergoing the examination.")
    premedication: List[str] = Field(..., description="List of premedications administered.")
    statistics: List[Statistics] = Field(..., description="Statistical data from the examination.")
    findings: List[Finding] = Field(..., description="List of findings observed during the examination.")
    notes: List[Note] = Field(..., description="List of notes recorded during the examination.")
    report_text: ReportText = Field(..., description="Full text content of the report.")
    references: List[str] = Field(..., description="List of references or external documents related to the report.")
    report_status: ReportStatus = Field(..., description="Current status of the report (e.g., final, preliminary).")
    additional_comment: str = Field(..., description="Additional comments or notes about the report.")
    last_change: str = Field(..., description="Timestamp of the last modification (ISO format).")
    issue_date: Optional[str] = Field(default_factory=lambda: datetime.now().isoformat(), description="Date and time when the report was issued (ISO format).")


class WorklistResponse(BaseModel):
    """Response structure for a worklist query."""
    uuid: str = Field(..., description="Unique identifier of the worklist response.")
    status: int = Field(..., description="Status code of the response (e.g., 200 for success).")
    patients: List[str] = Field(..., description="List of patient identifiers in the worklist.")
```