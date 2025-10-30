# Interview Tasks

## Prerequisites

- [Node.js & npm](https://nodejs.org/) (for Vue3)
- [.NET 9 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/9.0)
- [PostgreSQL](https://www.postgresql.org/)
- [Visual Studio 2022](https://visualstudio.microsoft.com/downloads/) (Optional)

## Vue3 - Installation

1. Install dependencies:
"npm install"
2. Start the development server:
"npm run dev"
3. Open your browser to the URL shown in the terminal (typically `http://localhost:5173`)

## Vue3 - Material design

- This project uses Vuetify as the material design: https://vuetifyjs.com/en/

## Products

- data.json

## Front-end Tasks - Vue 3

1. Add an image to each product.
1. Add a toggle button to switch the Available products from grid view to list view, displaying their respectful image next to it.
1. Add a remove button to each cart item. When click it should remove the item from the cart and add it back to the stock amount.
1. Add a sold out banner to the available products when the product stock is 0.
1. Add a modal popup for each product, to display the product details.

### Bonus

1. With the remove button in the cart, also add number input, that one can increase and decrease.
1. Auto apply discounts for total cost above certain amount: 
    * SAVE10 (above R3000) - 10% discount
    * SAVE15 (above R6000) - 15% discount
    * SAVE20 (above R9000) - 20% discount
1. There is a logic error in the code, find it and fix it!
1. Add cart functionality to the modal popup.

## Back-end Tasks - .net 9

- Create a back end to do the following:
    1. Get items
    1. Update items
    1. Log check outs
    1. Log product empty stock

### Bonus

1. Add an item expiry date, when it is expired, delete the item in the table.
1. Add email notification when stock is low.

## Database Tasks - Postgres

1. Create a user to access database
1. Create a database
1. Create relevent tables
1. Insert products to relevent tables

## Evaluation Criteria

- ✅ Code is clean
- ✅ Responsive layout
- ✅ UX improvements
- ✅ Attention to detail
- ✅ Excellent structures

## Submitting

- Include all relevant files and folder. 
- Please **do not** include `node_modules`.
- Dump your database with `pg_dump` 

> PS. If you are not able to send the zip with Gmail, create a share folder, and send us the link!

Good luck! 🚀