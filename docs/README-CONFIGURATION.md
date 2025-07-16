# Example of maia integration configuration

```
"communication_service_config": {
    "fhir": {
      "url": "fhir_url",
      "credentials": {
        "username": "username",
        "password": "password"
      },
      "location": "locationX"
    },
    "rest": {
      "url": "rest_url",
      "credentials": {
        "username": "username",
        "password": "password"
      },
      "location": "LocationX"
    },
    "mwl": {
        "host": "127.0.0.1",
        "port": 4242,
        "ae_title": "MAIA",
        "called_ae_title" : "WORKLIST_SERVER",
        "worklist_filter" : {
            "date": null,
            "modality": null,
            "station_aet": null
        }
    },
    "pacs": {
      "enabled": true,
      "communication_protocol": "dcmsend",
      "url_rest": "pacs_rest_url",
      "dicom_address": "dicom_address",
      "dicom_port": "port",
      "credentials": {
        "username": "username",
        "password": "password"
      },
      "location": "",
      "dicom_settings": {
        "manufacturer": "MAIA",
        "manufacturer_model_name": "Colo Maia",
        "modality": "ES",
        "specific_character_set": "ISO_IR 192",
        "series_description": "Endoscopic Examination",
        "requested_procedure_description": "Screening colonoscopy",
        "implementation_class_uid": "1.2.276.0.7230010.3.0.3.6.6",
        "video": {
          "transfer_syntax": "1.2.840.10008.1.2.4.102",
          "sop_class": "1.2.840.10008.5.1.4.1.1.77.1.4.1",
          "is_implicit_VR": false,
          "is_little_endian": true
        },
        "image": {
          "transfer_syntax": "1.2.840.10008.1.2.1",
          "sop_class": "1.2.840.10008.5.1.4.1.1.77.1.1",
          "is_implicit_VR": false,
          "is_little_endian": true
        }
      }
    },
    "report_server_type": "rest",
    "worklist_server_type" : "mwl",
  }
}
```