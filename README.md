# WCA Coding Challenge

This is the WCA coding challenege for the Full Stack Developer position.

### Challenge Goal:
Create an interactive chart showing how many Diabetic Foot Ulcers were treated in 2021 by state.
- The code should be committed to git and the git repo shared back via this email.
- Please include in readme file the steps to run locally. Sail, Homestead, Valet, php using 'artisan serve' - whatever process you use is fine. No database or other service will be required.
- Please use laravel for the backend, and vue for the frontend. Any other technologies are up to you. Barebones or other frameworks are fine.

### Challenge Details:
- Here is the [CMS API](https://data.cms.gov/provider-summary-by-type-of-service/medicare-physician-other-practitioners/medicare-physician-other-practitioners-by-geography-and-service) to use [api end point here](https://data.cms.gov/data-api/v1/dataset/6fea9d79-0129-4e4c-b1b8-23cd86a4f435/data)
- The title of the page should be something like "Medicare Services for Diabetic Foot Ulcer Debridements by State in 2021"
- The chart design can be any way that seems interesting - a simple bar chart is fine.
- Main variable should be state dropdown (others are fine too). You can default a state or require one before view.
- Main dimension (if you use bars for instance) should be the amount of services done for that year, and hover effect could show a few other things - mainly average submitted charges, and average medicare payments for that service.
- The HCPCS codes to use for diabetic foot ulcers are 11043, 11044, 11045, 11046, and 11047. Those should be filtered automatically.

### Getting Started
In your terminal:
```sh
cd app
npm install
php artisan serve
```
Open up another terminal, from inside app:
```
npm run dev
```

Open your browser to [http://localhost:8000](http://localhost:8000)

Enjoy 🎉