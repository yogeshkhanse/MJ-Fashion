document.addEventListener('DOMContentLoaded', function () {
    // Mock Clothing Items in LocalStorage
    let clothingItems = JSON.parse(localStorage.getItem('clothingItems')) || [
        { name: "T-Shirt", price: 20, image: 'images/default.jpg' },
        { name: "Jeans", price: 50, image: 'images/default.jpg' },
        { name: "Jacket", price: 80, image: 'images/default.jpg' },
        { name: "Sneakers", price: 60, image: 'images/default.jpg' },
        { name: "Dress", price: 100, image: 'images/default.jpg' },
        { name: "Shirt", price: 30, image: 'images/default.jpg' },
        { name: "Shorts", price: 25, image: 'images/default.jpg' },
        { name: "Hoodie", price: 75, image: 'images/default.jpg' },
        { name: "Cap", price: 15, image: 'images/default.jpg' }
    ];

    // Display Clothing Items on the page
    function displayClothingItems() {
        const clothingContainer = document.getElementById('clothing-items');
        clothingContainer.innerHTML = '';  // Clear any existing items
        clothingItems.forEach((item, index) => {
            const itemDiv = document.createElement('div');
            itemDiv.classList.add('item');
            itemDiv.innerHTML = `
                <img src="${item.image}" alt="${item.name}">
                <h3>${item.name}</h3>
                <p>Price: $${item.price}</p>
                <button onclick="editItem(${index})">Edit</button>
            `;
            clothingContainer.appendChild(itemDiv);
        });
    }

    // Admin Login
    const loginForm = document.getElementById('login-form');
    loginForm.addEventListener('submit', function (event) {
        event.preventDefault();
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;

        // Basic login validation
        if (username === 'admin' && password === 'admin123') {
            document.getElementById('admin-login').style.display = 'none';
            document.getElementById('admin-dashboard').style.display = 'block';
        } else {
            alert('Invalid credentials');
        }
    });

    // Admin Logout
    const logoutButton = document.getElementById('logout');
    logoutButton.addEventListener('click', function () {
        document.getElementById('admin-login').style.display = 'block';
        document.getElementById('admin-dashboard').style.display = 'none';
    });

    // Edit an item
    window.editItem = function (index) {
        const item = clothingItems[index];
        document.getElementById('item-name').value = item.name;
        document.getElementById('item-price').value = item.price;
        document.getElementById('item-image').value = ''; // Reset the file input
        document.getElementById('item-image').dataset.index = index;
    };

    // Update the clothing item (image and price)
    window.updateItem = function () {
        const name = document.getElementById('item-name').value;
        const price = parseFloat(document.getElementById('item-price').value);
        const imageInput = document.getElementById('item-image');
        const index = imageInput.dataset.index;
        
        // Handle image upload
        let imageURL = clothingItems[index].image;  // Default to existing image
        if (imageInput.files && imageInput.files[0]) {
            const reader = new FileReader();
            reader.onload = function (e) {
                imageURL = e.target.result;
                clothingItems[index] = { name, price, image: imageURL };
                localStorage.setItem('clothingItems', JSON.stringify(clothingItems));
                displayClothingItems();  // Refresh the clothing items display
            };
            reader.readAsDataURL(imageInput.files[0]);
        } else {
            // No image change, just update name and price
            clothingItems[index] = { name, price, image: imageURL };
            localStorage.setItem('clothingItems', JSON.stringify(clothingItems));
            displayClothingItems();
        }
    };

    // Initial load
    displayClothingItems();
});
