# Plex API Integration: Fusion 360 Tool Sync

`plex-api` is a project designed to automate the synchronization of tooling data between Autodesk Fusion 360 and the Plex Manufacturing Cloud (Rockwell Automation) for **Grace Engineering**.

## 🎯 Architecture & Primary Goal

The overarching goal of this project is to maintain an up-to-date tooling inventory without manual data entry. **Crucially, the Autodesk Fusion 360 tool library files act as the single source of truth for all tooling data entering Plex.**

**The 30,000-Foot View & Industry Standard Data Flow:**

1. **Source Data (Source of Truth)**: Autodesk Fusion 360 maintains a tool library stored as `.json` files on a local network share. 
2. **Component Hierarchy (Consumables First)**: In standard tooling management, Tool Assemblies are made up of purchased components. The script's initial focus is on the **consumable cutting tools** (e.g., end mills, drills) purchased from suppliers (tracked as purchased parts via POs).
3. **Plex Linkage & Traceability**: These consumable purchased parts are linked to Tool Assemblies. Tool Assemblies are then linked to specific Routings/Operations. When an operation runs on the shop floor, it generates a Job, which ultimately produces the manufactured Part.
4. **Scheduled Sync**: A script runs automatically every day at midnight.
5. **Plex Updates**: The script reads the Fusion 360 JSON files and pushes the data to Plex via its REST API, performing key actions:
   - Updates the tooling inventory in the master list, focusing on connecting purchased consumables to assemblies.
   - Updates the tooling in the respective workcenter document (`production/v1/control/workcenters`).
6. **Data Management**: For simplicity, state management and data files are maintained on the network shares using file overwriting.

## 📚 Resources

The Plex API is a modern, RESTful service utilizing JSON for data exchange. This integration will map local JSON structures to the cloud API. Note: Due to explicit IAM role permissions, certain Tooling endpoints are hidden from the developer portal until subscribed.

- **Official Documentation**: [Plex Manufacturing Cloud API](https://www.rockwellautomation.com/en-us/support/plex-manufacturing-cloud/api.html)
- **Project Roadmap**: See [TODO.md](./TODO.md) for step-by-step implementation tasks.
- **Reference**: See [Plex API Reference](./Plex_API_Reference.md) for endpoints, auth routing, and DNC details.
- **Data Mapping**: See [Fusion360 Reference](./Fusion360_Tool_Library_Reference.md) for data extraction rules.

## 🚀 Postman Testing

We use **[Postman](https://www.postman.com/)** for upfront API discovery and management demonstrations.

By saving queries to a Postman Collection, we can manually verify the exact structure needed to push inventory updates and workcenter document updates to Plex before writing the final automation script.
