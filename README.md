# Question Markup Language (QuML) Specification - Draft

Question Markup Language (QuML in short) is a specification for storage, rendering and distribution of Questions and Tests. QuML allows assessment materials to be authored and delivered on multiple systems interchangeably. It is designed to facilitate interoperability between systems.

This specification is based on [IMS QTI Specification 2.2](http://www.imsglobal.org/question/index.html#version2.2) with necessary extensions. This specification describes the information model for questions and tests used in assessments. Formally these are known as Question and Question Set. QuML specification describes a model for the representation of questions and question sets data and their corresponding results reports. Therefore, the specification enables the exchange of the question, question set and results data between authoring tools, question banks, test constructional tools, learning systems and assessment delivery systems.

## Documentation

- [**Overview**](https://github.com/sunbird-specs/inQuiry/blob/master/overview.md): Importance of questions in learning and the need for a specification like QuML.
- [**Common Elements**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/common.md): The reference guide to the data model of common elements used in this specification.
- [**QuML for Questions**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/question.md): The reference guide to the data model for questions. The document provides detailed information about the model and specifies the requirements of delivery engines and authoring systems.
- [**Question Schema**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/question-schema.json): JSON schema for QuML questions.
- [**QuML for Tests**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/questionSet.md): The reference guide to the data model for question sets, which are used to represent tests.
- [**Test Schema**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/question-set-schema.json): JSON schema for QuML tests.
- [**Library Methods**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/methods.md): The reference guide to methods to be implemented by QuML implementations.
- [**Telemetry**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/telemetry.md): The reference guide to track usage, and report results.
- [**Appendix**](https://github.com/sunbird-specs/inQuiry/blob/master/v1/appendix.md): Additional information and references.
- [**Samples**](https://github.com/sunbird-specs/inQuiry/tree/master/v1/samples): This folder has sample questions and question sets represented using QuML.

## Implementation(s)

The Sunbird [**knowledge-platform**](http://github.com/project-sunbird/knowledge-platform) using QuML Specification to enable Question and Quesion Set objects sourcing and consumption. Below is the current version of the schema defined to enable this.

- [**Question Schema**](v1/schemas/question/1.0/schema.json)
- [**Question Set Schema**](v1/schemas/questionset/1.0/schema.json)
