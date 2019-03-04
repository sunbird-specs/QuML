# Question Markup Language (QML) Specification - Draft

Question Markup Language (QML in short) is a specification for storage, rendering and distribution of Questions and Tests. QML allows assessment materials to be authored and delivered on multiple systems interchangeably. It is designed to facilitate interoperability between systems.

This specification describes the information model for questions and tests used in assessments. Formally these are known as the Question and Question Set. QML specification describes a model for the representation of questions and question sets data and their corresponding results reports. Therefore, the specification enables the exchange of the question, question set and results data between authoring tools, question banks, test constructional tools, learning systems and assessment delivery systems.

## Principles

### Formal Definition
A standard or a specification intended as such should rely as far as possible on existing and proven standards. This allows keeping the specification of the interchange format concise and enables implementers to make use of available and proven tools. The proposed model is primarily based on HTML, JSON and Javascript. 

> HTML, JSON and javascript, being well-defined and globally acknowledged standards, makes it simple for implementations to interpret and transfer questions and tests in the form intended by the original author.

### Composability
There are different scenarios for the interchange of tests. In some cases, tests may be reused in their entirety, whereas in other cases only individual questions from a test or a test collection (question bank) may be integrated into another test. It is therefore essential to clearly separate the different aspects of tests, especially individual questions and sets in a test.

### Size and Scope
A model that is able to describe all of the functionality of all systems may seem desirable to enable the interchange of complete tests with all their properties. Looking closer, one can see that this requirement is illusionary: The facilities of test systems are too diverse and too varied and no system supports all test and scoring types. A model should therefore restrict itself to a relatively small “core set” of test and question types. Further types can be added later on once it has become clear which types are actually required in practice. 

### Longevity
Finally, a specification should ensure longevity, i.e., questions and tests described using the format should remain processable for as long as possible. For correcting errors and extending the format, new revisions will become necessary from time to time, but it must be avoided that new revisions interfere with the data interchange. This means that different revisions should be compatible with each other as far as possible. Gratuitous incompatibilities must be avoided at all costs; sometimes, however, incompatible changes may be necessary.	 

To mitigate the potential negative impact of such changes, rigorous revision management is essential, starting with the distinction between major and minor revisions and corresponding numbering schemes. Incompatible changes must then only be introduced in major revisions, after having been announced before. Inside a major revision, say, 1.x, all minor revisions (e.g., 1.1, 1.2, etc.) are all compatible with each other. Revision 2.0 may introduce incompatible changes, but not 2.1. This ensures that implementers and users can easily and reliably decide whether a specific question or test can be processed or not.

## Structure of this specification
The specification is spread over a number of documents:

- [**Common Elements**](https://github.com/sunbird-specs/qml/blob/master/v1/common.md): The reference guide to the data model of common elements used in this specification.
- [**Question Information Model**](https://github.com/sunbird-specs/qml/blob/master/v1/question.md): The reference guide to the data model for questions. The document provides detailed information about the model and specifies the requirements of delivery engines and authoring systems.
- [**Question Set Information Model**](https://github.com/sunbird-specs/qml/blob/master/v1/questionSet.md): The reference guide to the data model for question sets, which are used to represent tests.
- [**Library Methods**](https://github.com/sunbird-specs/qml/blob/master/v1/methods.md): The reference guide to methods to be implemented by QML implementations.
- [**Samples**](https://github.com/sunbird-specs/qml/tree/master/v1/samples): This document has sample questions and question sets represented using QML.


