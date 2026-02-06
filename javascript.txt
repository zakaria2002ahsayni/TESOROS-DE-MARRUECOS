// Catálogo de Productos
const products = [
    {
        id: 1,
        name: "Alfombra Bereber Auténtica",
        description: "Tejida a mano por artesanos bereberes. 100% lana. 200x150cm",
        price: 3500,
        oldPrice: 4500,
        emoji: "🧵",
        category: "alfombras"
    },
    {
        id: 2,
        name: "Lámpara Marroquí de Latón",
        description: "Hecha a mano con técnica tradicional. Incluye bombilla LED",
        price: 850,
        oldPrice: 1200,
        emoji: "🏮",
        category: "decoracion"
    },
    {
        id: 3,
        name: "Set de Cerámica de Fez (6 piezas)",
        description: "Platos artesanales pintados a mano. Diseños únicos",
        price: 1200,
        oldPrice: 1600,
        emoji: "🏺",
        category: "ceramica"
    },
    {
        id: 4,
        name: "Babuchas de Cuero Premium",
        description: "Cuero genuino curtido artesanalmente. Tallas disponibles",
        price: 450,
        oldPrice: 650,
        emoji: "👞",
        category: "calzado"
    },
    {
        id: 5,
        name: "Tetera de Plata Grabada",
        description: "Plata 925 con grabados tradicionales. Capacidad 1.5L",
        price: 2800,
        oldPrice: 3500,
        emoji: "🫖",
        category: "menaje"
    },
    {
        id: 6,
        name: "Cojines Bordados (Set de 4)",
        description: "Bordado a mano con hilos de seda. 45x45cm cada uno",
        price: 680,
        oldPrice: 900,
        emoji: "🛋️",
        category: "textil"
    },
    {
        id: 7,
        name: "Espejo Tallado en Madera",
        description: "Madera de cedro tallada a mano. Marco decorativo 80x60cm",
        price: 1500,
        oldPrice: 2000,
        emoji: "🪞",
        category: "decoracion"
    },
    {
        id: 8,
        name: "Set de Especias Marroquíes",
        description: "7 especias tradicionales en recipientes artesanales",
        price: 350,
        oldPrice: 500,
        emoji: "🌶️",
        category: "gastronomia"
    },
    {
        id: 9,
        name: "Bolso de Cuero Artesanal",
        description: "Cuero genuino curtido naturalmente. Diseño tradicional",
        price: 950,
        oldPrice: 1300,
        emoji: "👜",
        category: "accesorios"
    },
    {
        id: 10,
        name: "Mesa de Té Plegable",
        description: "Madera y latón. Diseño tradicional marroquí. 60cm diámetro",
        price: 1800,
        oldPrice: 2400,
        emoji: "🪑",
        category: "muebles"
    },
    {
        id: 11,
        name: "Jabón de Argán Natural (Pack 6)",
        description: "100% aceite de argán. Hecho a mano sin químicos",
        price: 280,
        oldPrice: 400,
        emoji: "🧼",
        category: "cosmetica"
    },
    {
        id: 12,
        name: "Puف Marroquí de Cuero",
        description: "Cuero genuino relleno de algodón. Varios colores",
        price: 750,
        oldPrice: 1000,
        emoji: "🎨",
        category: "muebles"
    }
];

// Carrito de Compras
let cart = JSON.parse(localStorage.getItem('cart')) || [];

// Renderizar productos
function renderProducts() {
    const productGrid = document.getElementById('product-grid');
    
    products.forEach(product => {
        const productCard = document.createElement('div');
        productCard.className = 'product-card';
        productCard.innerHTML = `
            <div class="product-image">${product.emoji}</div>
            <div class="product-info">
                <h3>${product.name}</h3>
                <p>${product.description}</p>
                <div class="product-price">
                    <span class="price">${product.price} MAD</span>
                    <span class="old-price">${product.oldPrice} MAD</span>
                </div>
                <button class="add-to-cart" onclick="addToCart(${product.id})">
                    🛒 Añadir al Carrito
                </button>
            </div>
        `;
        productGrid.appendChild(productCard);
    });
}

// Añadir al carrito
function addToCart(productId) {
    const product = products.find(p => p.id === productId);
    const existingItem = cart.find(item => item.id === productId);
    
    if (existingItem) {
        existingItem.quantity += 1;
    } else {
        cart.push({
            ...product,
            quantity: 1
        });
    }
    
    updateCart();
    showNotification(`✅ ${product.name} añadido al carrito`);
}

// Actualizar carrito
function updateCart() {
    localStorage.setItem('cart', JSON.stringify(cart));
    document.getElementById('cart-count').textContent = cart.reduce((sum, item) => sum + item.quantity, 0);
    renderCartItems();
}

// Renderizar items del carrito
function renderCartItems() {
    const cartItemsContainer = document.getElementById('cart-items');
    
    if (cart.length === 0) {
        cartItemsContainer.innerHTML = '<p style="text-align: center; padding: 2rem;">Tu carrito está vacío</p>';
        document.getElementById('cart-total').textContent = '0';
        return;
    }
    
    cartItemsContainer.innerHTML = cart.map(item => `
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 1rem; border-bottom: 1px solid #ddd;">
            <div style="flex: 1;">
                <h4>${item.name}</h4>
                <p style="color: #666;">${item.price} MAD x ${item.quantity}</p>
            </div>
            <div style="display: flex; gap: 1rem; align-items: center;">
                <button onclick="changeQuantity(${item.id}, -1)" style="background: #ddd; border: none; padding: 0.5rem 1rem; border-radius: 5px; cursor: pointer;">-</button>
                <span>${item.quantity}</span>
                <button onclick="changeQuantity(${item.id}, 1)" style="background: #ddd; border: none; padding: 0.5rem 1rem; border-radius: 5px; cursor: pointer;">+</button>
                <button onclick="removeFromCart(${item.id})" style="background: #c41e3a; color: white; border: none; padding: 0.5rem 1rem; border-radius: 5px; cursor: pointer;">Eliminar</button>
            </div>
        </div>
    `).join('');
    
    const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    document.getElementById('cart-total').textContent = `${total} MAD`;
}

// Cambiar cantidad
function changeQuantity(productId, change) {
    const item = cart.find(item => item.id === productId);
    if (item) {
        item.quantity += change;
        if (item.quantity <= 0) {
            removeFromCart(productId);
        } else {
            updateCart();
        }
    }
}

// Eliminar del carrito
function removeFromCart(productId) {
    cart = cart.filter(item => item.id !== productId);
    updateCart();
}

// Mostrar modal del carrito
function showCart() {
    document.getElementById('cart-modal').style.display = 'block';
    renderCartItems();
}

// Cerrar modal
document.querySelector('.close').onclick = function() {
    document.getElementById('cart-modal').style.display = 'none';
}

window.onclick = function(event) {
    const modal = document.getElementById('cart-modal');
    if (event.target == modal) {
        modal.style.display = 'none';
    }
}

// Checkout
function checkout() {
    if (cart.length === 0) {
        alert('Tu carrito está vacío');
        return;
    }
    
    const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    const items = cart.map(item => `${item.name} x${item.quantity}`).join('\n');
    
    alert(`🎉 ¡Gracias por tu compra!\n\nResumen:\n${items}\n\nTotal: ${total} MAD\n\n📧 Recibirás un email de confirmación con los detalles del pago.\n📦 Envío estimado: 5-7 días laborables\n\n¡Te contactaremos pronto!`);
    
    // Vaciar carrito
    cart = [];
    updateCart();
    document.getElementById('cart-modal').style.display = 'none';
}

// Notificaciones
function showNotification(message) {
    const notification = document.createElement('div');
    notification.style.cssText = `
        position: fixed;
        top: 100px;
        right: 20px;
        background: #2ecc71;
        color: white;
        padding: 1rem 2rem;
        border-radius: 10px;
        box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        z-index: 3000;
        animation: slideIn 0.3s ease;
    `;
    notification.textContent = message;
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.style.animation = 'slideOut 0.3s ease';
        setTimeout(() => notification.remove(), 300);
    }, 3000);
}

// Formulario de contacto
document.getElementById('contact-form').addEventListener('submit', function(e) {
    e.preventDefault();
    showNotification('✅ Mensaje enviado. Te contactaremos pronto!');
    this.reset();
});

// Event listener para el botón del carrito
document.querySelector('.cart-btn').addEventListener('click', function(e) {
    e.preventDefault();
    showCart();
});

// Smooth scroll
document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function (e) {
        e.preventDefault();
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
            target.scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }
    });
});

// Animaciones CSS
const style = document.createElement('style');
style.textContent = `
    @keyframes slideIn {
        from {
            transform: translateX(400px);
            opacity: 0;
        }
        to {
            transform: translateX(0);
            opacity: 1;
        }
    }
    
    @keyframes slideOut {
        from {
            transform: translateX(0);
            opacity: 1;
        }
        to {
            transform: translateX(400px);
            opacity: 0;
        }
    }
`;
document.head.appendChild(style);

// Inicializar
window.addEventListener('DOMContentLoaded', () => {
    renderProducts();
    updateCart();
});
