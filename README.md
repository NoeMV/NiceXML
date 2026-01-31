# NiceXML

## Overview

NiceXML is a modern web application built with Vue 3, Vite, and Tailwind CSS v4, designed for parsing and displaying XML file contents in an interactive, user-friendly tree view. It allows users to easily upload XML files and explore their structure, elements, attributes, and values with collapsible nodes for better readability and navigation.

## Features

-   **XML File Upload:** Easily upload any `.xml` file from your local system.
-   **Interactive Tree View:** Visualize XML data in a hierarchical, collapsible tree structure.
-   **Element & Attribute Display:** Clearly shows XML element names, their attributes (with values), and text content.
-   **Collapsible Nodes:** Expand and collapse individual XML nodes to focus on relevant sections, with all non-root nodes collapsed by default for a clean initial view.
-   **Responsive Design:** Optimized for various screen sizes, ensuring a great user experience on both desktop and mobile devices.
-   **Scrollable Content:** Automatically handles large XML files with horizontal and vertical scrolling to prevent content overflow.

## Technologies Used

-   **Frontend Framework:** [Vue 3](https://vuejs.org/)
-   **Build Tool:** [Vite](https://vitejs.dev/)
-   **Styling:** [Tailwind CSS v4](https://tailwindcss.com/)
-   **XML Parsing:** [fast-xml-parser](https://github.com/NaturalIntelligence/fast-xml-parser)

## Setup and Installation

To get this project up and running on your local machine, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/NoeMV/NiceXML.git
    cd NiceXML
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Run the development server:**
    ```bash
    npm run dev
    ```
    The application will typically be available at `http://localhost:5173/` (or another port if 5173 is in use).

## Usage

1.  **Upload XML File:** Click the "Upload XML File" button. This will open your file explorer. Select an `.xml` file.
2.  **View Tree Structure:** The XML content will be parsed and displayed as a collapsible tree.
3.  **Navigate:** Click the arrow icons (► / ▼) next to each node to expand or collapse its children.
4.  **Upload Another File:** To load a new XML file, click the "Upload another file" button.

## Contributing

Contributions are welcome! If you have suggestions or want to improve the project, please open an issue or submit a pull request.

## License

This project is open-sourced under the [MIT License](https://opensource.org/licenses/MIT).