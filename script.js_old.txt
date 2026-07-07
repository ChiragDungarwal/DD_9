// Create floating particles in background
function createParticles() {
  const particlesContainer = document.createElement('div');
  particlesContainer.className = 'particles';
  document.body.appendChild(particlesContainer);
  
  const gridPattern = document.createElement('div');
  gridPattern.className = 'grid-pattern';
  document.body.appendChild(gridPattern);
  
  const particleCount = 50;
  
  for (let i = 0; i < particleCount; i++) {
    const particle = document.createElement('div');
    particle.className = 'particle';
    const size = Math.random() * 6 + 2;
    particle.style.width = `${size}px`;
    particle.style.height = `${size}px`;
    particle.style.left = `${Math.random() * 100}%`;
    const duration = Math.random() * 12 + 8;
    particle.style.animation = `float ${duration}s linear infinite`;
    particle.style.animationDelay = `${Math.random() * 10}s`;
    const colors = ['#1e3a8a', '#2563eb', '#3b82f6', '#60a5fa'];
    particle.style.background = colors[Math.floor(Math.random() * colors.length)];
    particle.style.opacity = Math.random() * 0.1 + 0.05;
    particlesContainer.appendChild(particle);
  }
}

// ========== SLIDESHOW FUNCTIONALITY ==========
function initSlideshow() {
  const slides = document.querySelectorAll('.slide');
  const prevBtn = document.querySelector('.slide-prev');
  const nextBtn = document.querySelector('.slide-next');
  const dotsContainer = document.querySelector('.slide-dots');
  let currentSlide = 0;
  let slideInterval;
  let isTransitioning = false;

  if (slides.length === 0) return;

  dotsContainer.innerHTML = '';
  slides.forEach((_, index) => {
    const dot = document.createElement('div');
    dot.classList.add('dot');
    if (index === 0) dot.classList.add('active');
    dot.addEventListener('click', () => {
      if (!isTransitioning) {
        goToSlide(index);
      }
    });
    dotsContainer.appendChild(dot);
  });

  const dots = document.querySelectorAll('.dot');

function goToSlide(index) {
  if (isTransitioning) return;
  if (index === currentSlide) return;
  
  isTransitioning = true;
  
  const leavingSlide = slides[currentSlide];
  
  slides[currentSlide].classList.remove('active');
  dots[currentSlide].classList.remove('active');
  
  currentSlide = index;
  
  slides[currentSlide].classList.add('active');
  dots[currentSlide].classList.add('active');
  
  const onTransitionEnd = () => {
    isTransitioning = false;
    leavingSlide.removeEventListener('transitionend', onTransitionEnd);
  };
  
  leavingSlide.addEventListener('transitionend', onTransitionEnd);
  
  // Fallback: if transitionend doesn't fire (e.g., no transition), unlock after 1.2s
  setTimeout(() => {
    if (isTransitioning) {
      isTransitioning = false;
      leavingSlide.removeEventListener('transitionend', onTransitionEnd);
    }
  }, 1100);
 }

  function nextSlide() {
    if (isTransitioning) return;
    let next = currentSlide + 1;
    if (next >= slides.length) {
      next = 0;
    }
    goToSlide(next);
    resetInterval();
  }

  function prevSlide() {
    if (isTransitioning) return;
    let prev = currentSlide - 1;
    if (prev < 0) {
      prev = slides.length - 1;
    }
    goToSlide(prev);
    resetInterval();
  }

  function resetInterval() {
    clearInterval(slideInterval);
    slideInterval = setInterval(nextSlide, 5000);
  }

  if (prevBtn) {
    prevBtn.addEventListener('click', prevSlide);
  }
  if (nextBtn) {
    nextBtn.addEventListener('click', nextSlide);
  }

  slideInterval = setInterval(nextSlide, 5000);

  const slideshowContainer = document.querySelector('.slideshow-container');
  if (slideshowContainer) {
    slideshowContainer.addEventListener('mouseenter', () => {
      clearInterval(slideInterval);
    });
    
    slideshowContainer.addEventListener('mouseleave', () => {
      slideInterval = setInterval(nextSlide, 5000);
    });
  }
}

// ========== FIXED: Dropdown toggles for nested structure ==========
function initDropdowns() {
  // Handle main product categories (BCH Products, Socomec Products)
  const categoryToggles = document.querySelectorAll('.category-toggle');
  categoryToggles.forEach(toggle => {
    toggle.removeEventListener('click', toggle._listener);
    const handler = (e) => {
      e.stopPropagation();
      const parent = toggle.closest('.product-category');
      if (parent) {
        parent.classList.toggle('active');
      }
    };
    toggle.addEventListener('click', handler);
    toggle._listener = handler;
  });
  
  // Handle ALL subcategories (including nested ones) - FIXED for independent toggling
  const allSubcategoryToggles = document.querySelectorAll('.subcategory-toggle');
  allSubcategoryToggles.forEach(toggle => {
    toggle.removeEventListener('click', toggle._listener);
    const handler = (e) => {
      e.stopPropagation();
      // Find the immediate parent .subcategory (not any parent)
      let parent = toggle.parentElement;
      while (parent && !parent.classList.contains('subcategory')) {
        parent = parent.parentElement;
      }
      if (parent) {
        parent.classList.toggle('active');
      }
    };
    toggle.addEventListener('click', handler);
    toggle._listener = handler;
  });
}

// ========== GLOBAL SEARCH FUNCTIONALITY WITH DEEP LINKING ==========

// Search index - contains all searchable content with location info
const searchIndex = [];

// Initialize search index with path information
function initializeSearchIndex() {
  searchIndex.length = 0;
  
  // ===== HOME PAGE =====
  searchIndex.push({
    page: 'index.html',
    title: 'Home',
    section: null,
    subsection: null,
    category: null,
    content: [
      'Dungarwal Distributors', 'Industrial Electrical Solutions', 'Quality Electrical Products',
      'Premium range of industrial electrical components from trusted brands',
      'BCH Electric Authorized Dealer', 'Genuine MCCB, MCB, Contactors and more',
      'Socomec Partner', 'High-quality power control and safety solutions',
      'About Us', 'Dungarwal Distributors is a trusted name'
    ]
  });
  
  // ===== BCH PRODUCTS - Pump Controllers =====
  searchIndex.push({
    page: 'products.html',
    title: 'DOL Starter',
    category: 'bch',
    section: 'Pump Controllers',
    subsection: null,
    anchor: 'dol-starter',
    content: ['DOL Starter', 'Direct-On-Line starter for simple and reliable motor starting applications']
  });
  
  searchIndex.push({
    page: 'products.html',
    title: 'FASD',
    category: 'bch',
    section: 'Pump Controllers',
    subsection: null,
    anchor: 'fasd',
    content: ['FASD', 'Fully automatic star-delta starters for efficient pump and motor control']
  });
 // ===== BCH PRODUCTS - Pump Controllers (NEW) =====

searchIndex.push({
  page: 'products.html',
  title: 'Motor Starter Panel',
  category: 'bch',
  section: 'Pump Controllers',
  subsection: null,
  anchor: 'motor-starter-panel',
  content: [
    'Motor Starter Panel',
    'SPS DOL and star delta starter panels',
    'starter panel',
    'motor control panel'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Single Phase Pump Panel',
  category: 'bch',
  section: 'Pump Controllers',
  subsection: null,
  anchor: 'single-phase-pump-panel',
  content: [
    'Single Phase Pump Panel',
    'Single phase pump panel',
    'pump controller',
    'single phase motor'
  ]
});
  
  // ===== BCH PRODUCTS - Power Control =====
  const powerControlProducts = [
    'Ultra Power Air Circuit Breakers', 'ACB', 'MCCB', 'MCB', 
    'MCB Distribution Box', 'SDF', 'RCCB'
  ];
  powerControlProducts.forEach(product => {
    searchIndex.push({
      page: 'products.html',
      title: product,
      category: 'bch',
      section: 'Industrial Products',
      subsection: 'Power Control',
      anchor: product.toLowerCase().replace(/ /g, '-'),
      content: [product, 'Power control products from BCH Electric']
    });
  });
  
  // ===== BCH PRODUCTS - Motor Control =====
  const motorControlProducts = [
    'Mini Contactors', 'Contactors', 'Special Purpose Contactors', 'Control Relay',
    'DC Contactors', 'Overload Relays', 'MPCB', 'Electronic Overload Relay', 'APFC', 'Power Capacitor'
  ];
  motorControlProducts.forEach(product => {
    searchIndex.push({
      page: 'products.html',
      title: product,
      category: 'bch',
      section: 'Industrial Products',
      subsection: 'Motor Control Products',
      anchor: product.toLowerCase().replace(/ /g, '-'),
      content: [product, 'Motor control products from BCH Electric']
    });
  });
  
  // ===== BCH PRODUCTS - Other Industrial =====
  const otherProducts = [
    'Control & Signalling Devices', 'Foot Switch', 'AC Solenoid & AC Brakes',
    'DC Brakes', 'Pneumatic Timers', 'Wires & Cables', 'Industrial Plug & Socket', 'Limit Switch', 'Proximity Sensor'
  ];
  otherProducts.forEach(product => {
    searchIndex.push({
      page: 'products.html',
      title: product,
      category: 'bch',
      section: 'Industrial Products',
      subsection: 'Other Industrial Products',
      anchor: product.toLowerCase().replace(/ /g, '-'),
      content: [product, 'Industrial products from BCH Electric']
    });
  });
  
  // ===== Enclosures =====
  searchIndex.push({
    page: 'products.html',
    title: 'Electrical Enclosures',
    category: 'bch',
    section: 'Enclosures',
    subsection: null,
    anchor: 'enclosures',
    content: ['Electrical Enclosures', 'Robust enclosures for protection of electrical components']
  });
  
// ===== SOCOMEC PRODUCTS =====
searchIndex.push({
  page: 'products.html',
  title: 'Automatic Transfer Switch',
  category: 'socomec',
  section: 'Power Switching',
  subsection: null,
  anchor: 'ats',
  content: [
    'Automatic Transfer Switch',
    'ATS',
    'power switching',
    'generator switching',
    'mains changeover'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Manual Changeover Switch',
  category: 'socomec',
  section: 'Power Switching',
  subsection: null,
  anchor: 'manual-changeover',
  content: [
    'Manual Changeover Switch',
    'changeover switch',
    'manual switching',
    'power transfer'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Switch Disconnectors',
  category: 'socomec',
  section: 'Power Switching',
  subsection: null,
  anchor: 'switch-disconnector',
  content: [
    'Switch Disconnectors',
    'isolator',
    'power isolation',
    'switching device'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Fuse Switch Disconnectors',
  category: 'socomec',
  section: 'Power Switching',
  subsection: null,
  anchor: 'fuse-switch',
  content: [
    'Fuse Switch Disconnectors',
    'fuse switch',
    'protection switch',
    'electrical safety'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Energy Meters',
  category: 'socomec',
  section: 'Power Monitoring',
  subsection: null,
  anchor: 'energy-meter',
  content: [
    'Energy Meters',
    'electric meter',
    'power consumption',
    'kWh meter'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Power Analyzers',
  category: 'socomec',
  section: 'Power Monitoring',
  subsection: null,
  anchor: 'power-analyzer',
  content: [
    'Power Analyzer',
    'power quality',
    'electrical analysis',
    'monitoring device'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Monitoring Systems',
  category: 'socomec',
  section: 'Power Monitoring',
  subsection: null,
  anchor: 'monitoring-system',
  content: [
    'Monitoring Systems',
    'energy monitoring',
    'real-time tracking',
    'power system'
  ]
});

searchIndex.push({
  page: 'products.html',
  title: 'Power Controllers',
  category: 'socomec',
  section: 'Power Monitoring',
  subsection: null,
  anchor: 'power-controller',
  content: [
    'Power Controllers',
    'energy control',
    'load control',
    'automation'
  ]
});
  
  // ===== CONTACT PAGE =====
  searchIndex.push({
    page: 'contact.html',
    title: 'Contact Us',
    category: null,
    section: null,
    subsection: null,
    anchor: 'contact',
    content: ['Contact Us', 'Get in touch', 'Email: info@dungarwal.com', 'Phone: +91 XXXXXXXXXX']
  });
}

// Function to expand dropdowns based on stored data (call this on products page)
function expandToProduct() {
  // Only run on products page
  if (!window.location.pathname.includes('products.html') && !window.location.pathname.endsWith('products')) {
    return;
  }
  
  const stored = sessionStorage.getItem('expandProduct');
  if (!stored) return;
  
  const expandData = JSON.parse(stored);
  
  // Clear after 2 seconds
  setTimeout(() => {
    sessionStorage.removeItem('expandProduct');
  }, 2000);
  
  // Expand main category (BCH or Socomec)
  const categories = document.querySelectorAll('.product-category');
  for (const cat of categories) {
    const toggle = cat.querySelector('.category-toggle');
    const catText = toggle ? toggle.querySelector('span')?.innerText || '' : '';
    
    if (expandData.category === 'bch' && catText.includes('BCH Products')) {
      cat.classList.add('active');
      
      if (expandData.section) {
        setTimeout(() => {
          expandSubcategory(cat, expandData.section, expandData.subsection, expandData.productTitle);
        }, 200);
      }
      break;
    } 
else if (expandData.category === 'socomec' && catText.includes('Socomec')) {
  cat.classList.add('active');

  // 🔥 ADD THIS LINE ONLY
  setTimeout(() => {
    expandSubcategory(cat, expandData.section, expandData.subsection, expandData.productTitle);
  }, 200);

  break;
}
  }
}

// Helper to expand subcategory and scroll to product
function expandSubcategory(categoryContainer, sectionName, subsectionName, productTitle) {
  const subcategories = categoryContainer.querySelectorAll('.subcategory');
  
  for (const subcat of subcategories) {
    const toggle = subcat.querySelector('.subcategory-toggle');
    const subcatText = toggle ? toggle.querySelector('span')?.innerText || '' : '';
    
    if (subcatText === sectionName) {
      subcat.classList.add('active');
      
      if (subsectionName) {
        setTimeout(() => {
          const nestedSubcats = subcat.querySelectorAll('.subcategory');
          for (const nested of nestedSubcats) {
            const nestedToggle = nested.querySelector('.subcategory-toggle');
            const nestedText = nestedToggle ? nestedToggle.querySelector('span')?.innerText || '' : '';
            if (nestedText === subsectionName) {
              nested.classList.add('active');
              
              setTimeout(() => {
                scrollToProduct(nested, productTitle);
              }, 300);
              break;
            }
          }
        }, 150);
      } else {
        setTimeout(() => {
          scrollToProduct(subcat, productTitle);
        }, 300);
      }
      break;
    }
  }
}

// Scroll to specific product card
function scrollToProduct(container, productTitle) {
  const productCards = container.querySelectorAll('.product-card');
  for (const card of productCards) {
    const titleElem = card.querySelector('h3');
    if (titleElem && titleElem.innerText.trim() === productTitle) {
      card.style.transition = 'all 0.3s ease';
      card.style.boxShadow = '0 0 0 3px #1e3a8a, 0 10px 25px rgba(0,0,0,0.1)';
      card.style.backgroundColor = '#f0f9ff';
      
      card.scrollIntoView({ behavior: 'smooth', block: 'center' });
      
      setTimeout(() => {
        card.style.boxShadow = '';
        card.style.backgroundColor = '';
      }, 2000);
      break;
    }
  }
}

// Search function
function performSearch(query) {
  if (!query || query.trim().length < 2) return [];
  
  const searchTerm = query.toLowerCase().trim();
  const results = [];
  
  for (const item of searchIndex) {
    let score = 0;
    let matchedContent = [];
    
    if (item.title.toLowerCase().includes(searchTerm)) {
      score += 10;
      matchedContent.push(item.title);
    }
    
    if (item.section && item.section.toLowerCase().includes(searchTerm)) {
      score += 5;
      matchedContent.push(`in ${item.section}`);
    }
    
    if (item.subsection && item.subsection.toLowerCase().includes(searchTerm)) {
      score += 4;
      matchedContent.push(`in ${item.subsection}`);
    }
    
    for (const content of item.content) {
      if (content.toLowerCase().includes(searchTerm)) {
        score += 3;
        let snippet = content;
        if (content.length > 60) snippet = content.substring(0, 60) + '...';
        matchedContent.push(snippet);
        break;
      }
    }
    
    if (score > 0) {
      results.push({
        page: item.page,
        title: item.title,
        section: item.section,
        subsection: item.subsection,
        category: item.category,
        score: score,
        matches: matchedContent.slice(0, 3)
      });
    }
  }
  
  results.sort((a, b) => b.score - a.score);
  return results;
}

// Create and inject search bar
function injectSearchBar() {
  const navContainer = document.querySelector('.main-nav .container');
  if (!navContainer) return;
  if (document.querySelector('.search-container')) return;
  
  const searchContainer = document.createElement('div');
  searchContainer.className = 'search-container';
  
  const searchWrapper = document.createElement('div');
  searchWrapper.className = 'search-wrapper';
  
  const searchIcon = document.createElement('i');
  searchIcon.className = 'fas fa-search search-icon';
  
  const searchInput = document.createElement('input');
  searchInput.type = 'text';
  searchInput.placeholder = 'Search products...';
  searchInput.className = 'search-input';
  searchInput.id = 'globalSearchInput';
  searchInput.autocomplete = 'off';
  
  const dropdown = document.createElement('div');
  dropdown.className = 'search-dropdown';
  dropdown.id = 'searchDropdown';
  
  searchWrapper.appendChild(searchIcon);
  searchWrapper.appendChild(searchInput);
  searchContainer.appendChild(searchWrapper);
  searchContainer.appendChild(dropdown);
  
  const navLinks = document.querySelector('.nav-links');
  const navToggle = document.querySelector('.nav-toggle');
  
  if (navLinks && navToggle) {
    navContainer.insertBefore(searchContainer, navToggle);
  } else if (navLinks) {
    navContainer.insertBefore(searchContainer, navLinks.nextSibling);
  } else {
    navContainer.appendChild(searchContainer);
  }
  
  searchInput.addEventListener('input', handleSearchInput);
  
  document.addEventListener('click', function(e) {
    if (!searchContainer.contains(e.target)) {
      dropdown.classList.remove('show');
    }
  });
}

let searchTimeout;
function handleSearchInput(e) {
  const query = e.target.value;
  const dropdown = document.getElementById('searchDropdown');
  
  clearTimeout(searchTimeout);
  
  searchTimeout = setTimeout(() => {
    if (query.length < 2) {
      dropdown.classList.remove('show');
      return;
    }
    const results = performSearch(query);
    displaySearchResults(results, query);
  }, 250);
}

function displaySearchResults(results, query) {
  const dropdown = document.getElementById('searchDropdown');
  if (!dropdown) return;
  
  if (results.length === 0) {
    dropdown.innerHTML = '<div class="search-no-results">🔍 No results found for "' + escapeHtml(query) + '"</div>';
    dropdown.classList.add('show');
    return;
  }
  
  let html = '';
  for (const result of results) {
    const badge = result.section ? `<span class="result-badge">${escapeHtml(result.section)}</span>` : '';
    html += `
      <div class="search-result-item" data-page="${result.page}" data-title="${escapeHtml(result.title)}" data-section="${result.section || ''}" data-subsection="${result.subsection || ''}" data-category="${result.category || ''}">
        <div class="search-result-title">
          <i class="fas ${getPageIcon(result.page)}"></i>
          <strong>${escapeHtml(result.title)}</strong>
          ${badge}
        </div>
        <div class="search-result-matches">
          ${result.matches.map(m => `<span class="search-match">${escapeHtml(m)}</span>`).join('')}
        </div>
        <div class="search-result-link">
          Click to open directly <i class="fas fa-arrow-right"></i>
        </div>
      </div>
    `;
  }
  
  dropdown.innerHTML = html;
  dropdown.classList.add('show');
  
  const resultItems = dropdown.querySelectorAll('.search-result-item');
  resultItems.forEach(item => {
    item.addEventListener('click', function(e) {
      e.preventDefault();
      const page = this.dataset.page;
      const title = this.dataset.title;
      const section = this.dataset.section;
      const subsection = this.dataset.subsection;
      const category = this.dataset.category;
      
      const expandData = {
        productTitle: title,
        section: section,
        subsection: subsection,
        category: category,
        timestamp: Date.now()
      };
      sessionStorage.setItem('expandProduct', JSON.stringify(expandData));
      
      window.location.href = page;
    });
  });
}

function getPageIcon(page) {
  if (page.includes('index')) return 'fa-home';
  if (page.includes('product')) return 'fa-box';
  if (page.includes('contact')) return 'fa-envelope';
  return 'fa-file-alt';
}

function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}

// Initialize search
function initGlobalSearch() {
  initializeSearchIndex();
  injectSearchBar();
  expandToProduct();
}

// Form to WhatsApp
function sendToWhatsApp() {
  let name = document.getElementById('name')?.value;
  let phone = document.getElementById('phone')?.value;
  let msg = document.getElementById('message')?.value;
  
  if (!name || !phone || !msg) {
    alert('Please fill all fields');
    return;
  }
  
  let url = `https://wa.me/919950136568?text=Name:%20${name}%0APhone:%20${phone}%0ARequirement:%20${msg}`;
  window.open(url, '_blank');
}

// Scroll Animation
const faders = document.querySelectorAll('.fade-up');
window.addEventListener('scroll', () => {
  faders.forEach(el => {
    const top = el.getBoundingClientRect().top;
    if (top < window.innerHeight - 50) {
      el.style.opacity = '1';
      el.style.transform = 'translateY(0)';
    }
  });
});

// ========== PRODUCT CARD BUTTON FUNCTIONALITY ==========
// Makes each product card clickable and opens its individual HTML file

function makeProductsClickable() {
  // Get all product cards on the page
  const productCards = document.querySelectorAll('.product-card');
  
  if (productCards.length === 0) return;
  
  // Function to generate filename from product name
  function getProductFileName(productName) {
    // Remove any extra spaces and trim
    let cleanName = productName.trim();
    
    // Manual mapping for specific products (customize as needed)
    const customMappings = {
      "DOL Starter": "dol-starter.html",
      "FASD": "fasd.html",
      "Motor Starter Panel": "motor-starter-panel.html",
      "Single Phase Pump Panel": "single-phase-pump-panel.html",
      "MCCB": "mccb.html",
      "MCB": "mcb.html",
      "MCB Distribution Box": "mcb-distribution-box.html",
      "SDF": "sdf.html",
      "ACB": "acb.html",
      "RCCB": "rccb.html",
      "Mini Contactors": "mini-contactors.html",
      "Contactors": "contactors.html",
      "Special Purpose Contactors": "special-purpose-contactors.html",
      "Control Relay": "control-relay.html",
      "DC Contactors": "dc-contactors.html",
      "Overload Relays": "overload-relays.html",
      "MPCB": "mpcb.html",
      "Electronic Overload Relay": "electronic-overload-relay.html",
      "APFC": "apfc.html",
      "Power Capacitor": "power-capacitor.html",
      "Control & Signalling Devices": "control-signalling-devices.html",
      "Foot Switch": "foot-switch.html",
      "AC Solenoid & AC Brakes": "ac-solenoid-brakes.html",
      "DC Brakes": "dc-brakes.html",
      "Pneumatic Timers": "pneumatic-timers.html",
      "Wires & Cables": "wires-cables.html",
      "Industrial Plug & Socket": "industrial-plug-socket.html",
      "Limit Switch": "limit-switch.html",
      "Proximity Sensor": "proximity-sensor.html",
      "Electrical Enclosures": "electrical-enclosures.html",
      "Automatic Transfer Switch": "automatic-transfer-switch.html",
      "Manual Changeover Switch": "manual-changeover-switch.html",
      "Switch Disconnectors": "switch-disconnectors.html",
      "Fuse Switch Disconnectors": "fuse-switch-disconnectors.html",
      "Energy Meters": "energy-meters.html",
      "Power Analyzers": "power-analyzers.html",
      "Monitoring Systems": "monitoring-systems.html",
      "Power Controllers": "power-controllers.html"
    };
    
    // Check if there's a custom mapping
    if (customMappings[cleanName]) {
      return customMappings[cleanName];
    }
    
    // Default: convert to lowercase, replace spaces with hyphens, remove special characters
    let fileName = cleanName.toLowerCase()
      .replace(/[&\/\\#,+()$~%.'":*?<>{}]/g, '') // Remove special chars
      .replace(/\s+/g, '-'); // Replace spaces with hyphens
    
    return fileName + '.html';
  }
  
  // Make each product card clickable
  productCards.forEach(card => {
    // Make it look clickable
    card.style.cursor = 'pointer';
    card.setAttribute('role', 'button');
    card.setAttribute('tabindex', '0');
    
    // Get product name from h3 tag
    const titleElement = card.querySelector('h3');
    
    if (titleElement) {
      const productName = titleElement.textContent.trim();
      const fileName = getProductFileName(productName);
      
      // Add click event to open the product's HTML file
      card.addEventListener('click', function(e) {
        e.stopPropagation();
        
        // Navigate to the product's individual HTML file
        window.location.href = fileName;
      });
      
      // Add keyboard support for accessibility (Enter and Space keys)
      card.addEventListener('keypress', function(e) {
        if (e.key === 'Enter' || e.key === ' ') {
          e.preventDefault();
          window.location.href = fileName;
        }
      });
      
      // Add hover effect title to indicate clickable
      card.setAttribute('title', `Click to view ${productName} details`);
    }
  });
  
  // Add a small style enhancement for product cards
  const style = document.createElement('style');
  style.textContent = `
    .product-card {
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }
    .product-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 25px rgba(0,0,0,0.1);
    }
    .product-card:active {
      transform: translateY(-2px);
    }
  `;
  document.head.appendChild(style);
}

// ========== MAIN DOM CONTENT LOADED EVENT ==========
document.addEventListener('DOMContentLoaded', () => {
  createParticles();
  initSlideshow();
  initGlobalSearch();
  initDropdowns(); // FIXED: This handles nested dropdowns properly
  
  
  // Initialize product card clickability
  makeProductsClickable();
  
  const navToggle = document.querySelector('.nav-toggle');
  const navLinks = document.querySelector('.nav-links');
  
  if (navToggle) {
    navToggle.addEventListener('click', () => {
      navLinks.classList.toggle('active');
    });
  }
  
  // Close mobile menu when clicking a link
  const navLinksItems = document.querySelectorAll('.nav-links a');
  navLinksItems.forEach(link => {
    link.addEventListener('click', () => {
      navLinks.classList.remove('active');
    });
  });
});