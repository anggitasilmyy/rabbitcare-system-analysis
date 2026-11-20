# RabbitCare PlantUML Diagrams

This directory contains 20 editable PlantUML source diagrams for the RabbitCare system analysis and design.

The individual `.puml` files are the primary editable sources. The diagram set is grouped by modeling purpose below.

## Diagram Index

| File | Diagram Type | Purpose |
|---|---|---|
| `01_use_case_context.puml` | Use Case | System context and primary actors. |
| `02_use_case_owner_portal.puml` | Use Case | Owner Portal features and interactions. |
| `03_use_case_clinic_operations.puml` | Use Case | Clinic operations and staff interactions. |
| `04_activity_registration_profile.puml` | Activity | Owner registration and rabbit profile creation. |
| `05_activity_booking_service.puml` | Activity | Service booking workflow. |
| `06_activity_clinical_visit.puml` | Activity | Patient arrival, check-in, and clinical examination. |
| `07_activity_vaccination.puml` | Activity | Vaccination workflow and pre-vaccination assessment. |
| `08_activity_grooming.puml` | Activity | Grooming workflow and initial safety assessment. |
| `09_activity_payment.puml` | Activity | Billing and payment workflow. |
| `10_activity_reminder.puml` | Activity | Care reminder workflow. |
| `11_sequence_booking.puml` | Sequence | Interactions involved in service booking. |
| `12_sequence_clinical_exam.puml` | Sequence | Check-in and clinical examination interactions. |
| `13_sequence_vaccination.puml` | Sequence | Vaccination interactions. |
| `14_sequence_grooming.puml` | Sequence | Grooming interactions. |
| `15_sequence_payment.puml` | Sequence | Billing and payment interactions. |
| `16_sequence_reminder.puml` | Sequence | Reminder and rebooking interactions. |
| `17_class_diagram.puml` | Class | Core domain classes and relationships. |
| `18_state_booking.puml` | State Machine | Booking lifecycle and status transitions. |
| `19_component_diagram.puml` | Component | Logical application components and dependencies. |
| `20_deployment_diagram.puml` | Deployment | Proposed deployment nodes and connections. |

## Source and Rendering Notes

- Keep the individual `.puml` files as the editable source of truth.
- The current repository snapshot contains PlantUML source files; rendered PNG/SVG previews have not yet been added.
- Presentation-ready exports can be added in a later step after the diagrams have been refined and visually checked.
- The booking and payment diagrams should be reviewed against the approved refinement decisions before being treated as the final workflow.

## Diagram Categories

- **Use Case:** system boundary, actors, and user-facing capabilities.
- **Activity:** end-to-end process flows and decision points.
- **Sequence:** interactions between actors, interfaces, services, and data stores.
- **Class:** conceptual domain entities, attributes, and relationships.
- **State Machine:** lifecycle and transitions of a booking.
- **Component:** logical application modules and dependencies.
- **Deployment:** proposed runtime nodes and communication paths.
