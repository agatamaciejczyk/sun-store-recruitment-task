# sun.store - Product Catalog

A modern, responsive product catalog application built with Vue 3, TypeScript, and Tailwind CSS. Features advanced filtering, search functionality, and persistent URL-based state management.

## Live Demo

**[https://sun-store-recruitment-task.vercel.app/](https://sun-store-recruitment-task.vercel.app/)**

## Features

- **Product Display**: Clean card-based layout showing product details (name, category, manufacturer, description, price)
- **Search**: Real-time search across product name, manufacturer, and description
- **Filters**:
  - Category filter with button-based selection
  - Manufacturer filter
  - Price range filter (min/max inputs with reset functionality)
- **Pagination**: Smart pagination with ellipsis, mobile-responsive design
- **URL State Management**: All filters and pagination state persist in URL query parameters
- **Responsive Design**: Mobile-first approach with sidebar on desktop, stacked layout on mobile
- **Sorting**: Products automatically sorted by price (lowest to highest)

## Tech Stack

- **Vue 3**: Composition API with `<script setup>` syntax
- **TypeScript**: Full type safety 
- **Tailwind CSS**: Utility-first styling with custom components
- **Vite**: Fast build tool and dev server
- **PapaParse**: CSV data parsing
- **Lucide Vue Next**: Modern icon library
- **Vitest**: Unit testing framework

## Installation

### Prerequisites

- Node.js 20.19.4

### Setup

```sh
npm install
```

### Development

```sh
npm run dev
```

### Build for Production

```sh
npm run build
```

### Preview Production Build

```sh
npm run preview
```
