# Theme Wording System

Sistem manajemen teks/wording untuk theme dengan admin panel.

## Struktur Folder

```
/inc/wording/
├── wording-init.php        # Loader utama (include di functions.php)
├── wording-fields.php      # Load semua page files
├── wording-admin.php       # Admin panel
├── wording-helper.php      # Helper functions
├── README.md               # Dokumentasi ini
└── pages/                  # Folder page wording
    ├── _template.php       # Template untuk page baru
    ├── global.php          # Wording global
    ├── 3d2n-komodo-tour.php
    └── field-types-demo.php
```

---

## Quick Start - Penggunaan di Template

### 1. Cara Cepat (Shorthand)
```php
// Text
we('field_key');                    // Echo escaped
$val = w('field_key');              // Get value

// URL
we_url('link_url');

// WYSIWYG / HTML
we_html('content_field');

// Image dengan fallback
we_img('upload_field', 'url_field');

// Repeater
foreach (w_repeater('items') as $item) {
    echo $item['title'];
}

// Checkbox
if (w_checked('show_feature')) { ... }
```

### 2. Menggunakan Template Variables
```php
// Di awal template, load wording
$w = wording_3d2n_komodo();

// Gunakan seperti array biasa
echo esc_html($w['hero']['title']);
echo esc_url($w['hero']['video_url']);
echo wp_kses_post($w['form']['title']);

// Loop repeater
foreach ($w['features'] as $feature) {
    echo $feature['title'];
}
```

---

## Membuat Page Wording Baru

### Step 1: Copy Template
```bash
cp pages/_template.php pages/your-page.php
```

### Step 2: Edit File
Ganti `PAGENAME` dengan nama page Anda:

```php
<?php
// File: pages/contact.php

function wording_page_contact_fields() {
    return [
        'title' => 'Contact Page',
        'fields' => [
            'contact_title' => [
                'label'   => 'Page Title',
                'type'    => 'text',
                'default' => 'Contact Us',
                'group'   => 'Hero',
            ],
            // ... fields lainnya
        ],
    ];
}

function wording_contact() {
    return [
        'title' => w('contact_title'),
        // ... mapping lainnya
    ];
}
```

### Step 3: Done!
File akan otomatis ter-register. Tidak perlu edit `wording-fields.php`.

Sistem akan auto-detect semua function dengan pattern `wording_page_{name}_fields()`.

---

## Daftar Shorthand Functions

| Function | Kegunaan | Contoh |
|----------|----------|--------|
| `w($key)` | Get value | `$val = w('title')` |
| `we($key)` | Echo text (escaped) | `we('title')` |
| `we_url($key)` | Echo URL | `we_url('link')` |
| `we_html($key)` | Echo HTML (WYSIWYG) | `we_html('content')` |
| `w_repeater($key)` | Get repeater array | `foreach(w_repeater('items'))` |
| `w_img($up, $url)` | Get image URL with fallback | `w_img('upload', 'url')` |
| `we_img($up, $url)` | Echo image URL | `we_img('upload', 'url')` |
| `w_checked($key)` | Check checkbox (bool) | `if(w_checked('show'))` |
| `w_img_tag($key)` | Output img tag | `w_img_tag('img', 'class')` |

---

## Field Types yang Tersedia

| Type | Deskripsi | Contoh Config |
|------|-----------|---------------|
| `text` | Input satu baris | `'type' => 'text'` |
| `textarea` | Input multi-baris | `'type' => 'textarea'` |
| `wysiwyg` | Visual editor | `'type' => 'wysiwyg'` |
| `url` | Input URL | `'type' => 'url'` |
| `image` | Media Library picker | `'type' => 'image'` |
| `image_url` | External image URL | `'type' => 'image_url'` |
| `number` | Input angka | `'type' => 'number', 'min' => 0, 'max' => 100` |
| `select` | Dropdown | `'type' => 'select', 'choices' => [...]` |
| `checkbox` | Boolean | `'type' => 'checkbox'` |
| `repeater` | Multiple items | Lihat contoh di bawah |

---

## Contoh Field Configurations

### Text dengan Default
```php
'hero_title' => [
    'label'   => 'Hero Title',
    'type'    => 'text',
    'default' => 'Welcome',
    'group'   => 'Hero Section',
],
```

### WYSIWYG
```php
'content' => [
    'label' => 'Content',
    'type'  => 'wysiwyg',
    'group' => 'Content',
],
```

### Image dengan Fallback
```php
'hero_image' => [
    'label' => 'Hero Image (Upload)',
    'type'  => 'image',
    'group' => 'Hero',
],
'hero_image_url' => [
    'label'   => 'Hero Image (URL Fallback)',
    'type'    => 'image_url',
    'default' => '/path/to/default.jpg',
    'group'   => 'Hero',
],
```

### Simple Repeater
```php
'features' => [
    'label' => 'Features',
    'type'  => 'repeater',
    'group' => 'Features',
],
```

### Complex Repeater
```php
'slides' => [
    'label'     => 'Slides',
    'type'      => 'repeater',
    'subfields' => [
        'title' => 'Title',
        'image' => [
            'label' => 'Image',
            'type'  => 'image',
        ],
        'content' => [
            'label' => 'Content',
            'type'  => 'wysiwyg',
        ],
    ],
    'group' => 'Slider',
],
```

### Nested Repeater
```php
'sections' => [
    'label'     => 'Sections',
    'type'      => 'repeater',
    'subfields' => [
        'title' => 'Section Title',
        'items' => [
            'label'     => 'Items',
            'type'      => 'repeater',
            'subfields' => [
                'name' => 'Name',
                'icon' => [
                    'label' => 'Icon',
                    'type'  => 'image',
                ],
            ],
        ],
    ],
],
```

---

## Penggunaan di Template (Lengkap)

### Contoh: 3D2N Komodo Tour
```php
<?php
// template-parts/3d2n-komodo-tour.php

// Load wording
$w = wording_3d2n_komodo();
?>

<!-- Hero -->
<section class="hero">
    <h1><?php echo esc_html($w['hero']['title']); ?></h1>
    <a href="<?php echo esc_url($w['hero']['tripadvisor_url']); ?>">
        <?php echo esc_html($w['hero']['tripadvisor_text']); ?>
    </a>
</section>

<!-- Info Bar -->
<div class="info-bar">
    <div class="info-item">
        <span class="label"><?php echo esc_html($w['info_bar']['price_label']); ?></span>
        <span class="value"><?php echo esc_html($w['info_bar']['price_value']); ?></span>
    </div>
</div>

<!-- Maps -->
<section class="maps">
    <h2><?php echo esc_html($w['maps']['title']); ?></h2>
    <img src="<?php echo esc_url($w['maps']['image_url']); ?>"
         alt="<?php echo esc_attr($w['maps']['alt_text']); ?>">
</section>

<!-- Form -->
<section class="form-section"
         style="background-image: url('<?php echo esc_url($w['form']['background_image']); ?>')">
    <h2><?php echo wp_kses_post($w['form']['title']); ?></h2>
</section>
```

### Atau Langsung dengan Shorthand
```php
<section class="hero">
    <h1><?php we('3d2n_hero_title'); ?></h1>
    <a href="<?php we_url('3d2n_hero_tripadvisor_url'); ?>">
        <?php we('3d2n_hero_tripadvisor_text'); ?>
    </a>
</section>
```

---

## Implementasi Lengkap ke Page Template

Panduan step-by-step untuk mengimplementasikan wording system ke page template WordPress.

### Step 1: Buat File Wording

Buat file baru di `/inc/wording/pages/`:

```php
<?php
// File: inc/wording/pages/about-us.php

if (!defined('ABSPATH')) {
    exit;
}

/**
 * Admin Fields - Ditampilkan di WP Admin
 */
function wording_page_about_us_fields() {
    return [
        'title' => 'About Us Page',
        'fields' => [

            // Hero Section
            'about_hero_title' => [
                'label'   => 'Hero Title',
                'type'    => 'text',
                'default' => 'About Us',
                'group'   => 'Hero',
            ],
            'about_hero_subtitle' => [
                'label'   => 'Hero Subtitle',
                'type'    => 'textarea',
                'default' => 'Learn more about our company',
                'group'   => 'Hero',
            ],
            'about_hero_image' => [
                'label' => 'Hero Background (Upload)',
                'type'  => 'image',
                'group' => 'Hero',
            ],
            'about_hero_image_url' => [
                'label'   => 'Hero Background (URL Fallback)',
                'type'    => 'image_url',
                'default' => '/wp-content/themes/flavor/assets/images/about-hero.jpg',
                'group'   => 'Hero',
            ],

            // Content Section
            'about_content_title' => [
                'label'   => 'Content Title',
                'type'    => 'text',
                'default' => 'Our Story',
                'group'   => 'Content',
            ],
            'about_content_body' => [
                'label' => 'Content Body',
                'type'  => 'wysiwyg',
                'group' => 'Content',
            ],

            // Team Section
            'about_team_title' => [
                'label'   => 'Team Section Title',
                'type'    => 'text',
                'default' => 'Meet Our Team',
                'group'   => 'Team',
            ],
            'about_team_members' => [
                'label'     => 'Team Members',
                'type'      => 'repeater',
                'subfields' => [
                    'name'     => 'Name',
                    'position' => 'Position',
                    'photo'    => [
                        'label' => 'Photo',
                        'type'  => 'image',
                    ],
                    'bio' => [
                        'label' => 'Bio',
                        'type'  => 'textarea',
                    ],
                ],
                'group' => 'Team',
            ],

            // CTA Section
            'about_cta_title' => [
                'label'   => 'CTA Title',
                'type'    => 'text',
                'default' => 'Ready to Get Started?',
                'group'   => 'CTA',
            ],
            'about_cta_button_text' => [
                'label'   => 'CTA Button Text',
                'type'    => 'text',
                'default' => 'Contact Us',
                'group'   => 'CTA',
            ],
            'about_cta_button_url' => [
                'label'   => 'CTA Button URL',
                'type'    => 'url',
                'default' => '/contact',
                'group'   => 'CTA',
            ],

        ],
    ];
}

/**
 * Template Variables - Untuk digunakan di template
 */
function wording_about_us() {
    return [
        'hero' => [
            'title'      => w('about_hero_title'),
            'subtitle'   => w('about_hero_subtitle'),
            'background' => w_img('about_hero_image', 'about_hero_image_url'),
        ],
        'content' => [
            'title' => w('about_content_title'),
            'body'  => w('about_content_body'),
        ],
        'team' => [
            'title'   => w('about_team_title'),
            'members' => w_repeater('about_team_members'),
        ],
        'cta' => [
            'title'       => w('about_cta_title'),
            'button_text' => w('about_cta_button_text'),
            'button_url'  => w('about_cta_button_url'),
        ],
    ];
}
```

### Step 2: Buat Page Template

Buat file template di root theme atau `/template-parts/`:

```php
<?php
/**
 * Template Name: About Us
 * File: page-about-us.php
 */

get_header();

// Load wording
$w = wording_about_us();
?>

<!-- Hero Section -->
<section class="hero" style="background-image: url('<?php echo esc_url($w['hero']['background']); ?>')">
    <div class="container">
        <h1><?php echo esc_html($w['hero']['title']); ?></h1>
        <p><?php echo esc_html($w['hero']['subtitle']); ?></p>
    </div>
</section>

<!-- Content Section -->
<section class="content-section">
    <div class="container">
        <h2><?php echo esc_html($w['content']['title']); ?></h2>
        <div class="content-body">
            <?php echo wp_kses_post($w['content']['body']); ?>
        </div>
    </div>
</section>

<!-- Team Section -->
<section class="team-section">
    <div class="container">
        <h2><?php echo esc_html($w['team']['title']); ?></h2>

        <?php if (!empty($w['team']['members'])) : ?>
            <div class="team-grid">
                <?php foreach ($w['team']['members'] as $member) : ?>
                    <div class="team-member">
                        <?php if (!empty($member['photo'])) : ?>
                            <img src="<?php echo esc_url($member['photo']); ?>"
                                 alt="<?php echo esc_attr($member['name']); ?>">
                        <?php endif; ?>
                        <h3><?php echo esc_html($member['name']); ?></h3>
                        <span class="position"><?php echo esc_html($member['position']); ?></span>
                        <p><?php echo esc_html($member['bio']); ?></p>
                    </div>
                <?php endforeach; ?>
            </div>
        <?php endif; ?>
    </div>
</section>

<!-- CTA Section -->
<section class="cta-section">
    <div class="container">
        <h2><?php echo esc_html($w['cta']['title']); ?></h2>
        <a href="<?php echo esc_url($w['cta']['button_url']); ?>" class="btn btn-primary">
            <?php echo esc_html($w['cta']['button_text']); ?>
        </a>
    </div>
</section>

<?php get_footer(); ?>
```

### Step 3: Alternatif - Langsung dengan Shorthand

Jika tidak ingin menggunakan template variables, bisa langsung pakai shorthand:

```php
<?php
/**
 * Template Name: About Us (Shorthand)
 */

get_header();
?>

<!-- Hero Section -->
<section class="hero" style="background-image: url('<?php we_img('about_hero_image', 'about_hero_image_url'); ?>')">
    <div class="container">
        <h1><?php we('about_hero_title'); ?></h1>
        <p><?php we('about_hero_subtitle'); ?></p>
    </div>
</section>

<!-- Content Section -->
<section class="content-section">
    <div class="container">
        <h2><?php we('about_content_title'); ?></h2>
        <div class="content-body">
            <?php we_html('about_content_body'); ?>
        </div>
    </div>
</section>

<!-- Team Section -->
<section class="team-section">
    <div class="container">
        <h2><?php we('about_team_title'); ?></h2>

        <?php $team = w_repeater('about_team_members'); ?>
        <?php if (!empty($team)) : ?>
            <div class="team-grid">
                <?php foreach ($team as $member) : ?>
                    <div class="team-member">
                        <?php if (!empty($member['photo'])) : ?>
                            <img src="<?php echo esc_url($member['photo']); ?>"
                                 alt="<?php echo esc_attr($member['name']); ?>">
                        <?php endif; ?>
                        <h3><?php echo esc_html($member['name']); ?></h3>
                        <span class="position"><?php echo esc_html($member['position']); ?></span>
                        <p><?php echo esc_html($member['bio']); ?></p>
                    </div>
                <?php endforeach; ?>
            </div>
        <?php endif; ?>
    </div>
</section>

<!-- CTA Section -->
<section class="cta-section">
    <div class="container">
        <h2><?php we('about_cta_title'); ?></h2>
        <a href="<?php we_url('about_cta_button_url'); ?>" class="btn btn-primary">
            <?php we('about_cta_button_text'); ?>
        </a>
    </div>
</section>

<?php get_footer(); ?>
```

---

## Cara Menggunakan di Template Part

Jika menggunakan `get_template_part()`:

### File utama (page-about.php):
```php
<?php
get_header();

// Load wording sekali di awal
$GLOBALS['wording_about'] = wording_about_us();

get_template_part('template-parts/about/hero');
get_template_part('template-parts/about/content');
get_template_part('template-parts/about/team');

get_footer();
```

### Template part (template-parts/about/hero.php):
```php
<?php
$w = $GLOBALS['wording_about'];
?>
<section class="hero" style="background-image: url('<?php echo esc_url($w['hero']['background']); ?>')">
    <h1><?php echo esc_html($w['hero']['title']); ?></h1>
</section>
```

---

## Tips & Best Practices

### 1. Naming Convention
Gunakan prefix yang konsisten untuk setiap page:
- `about_` untuk About page
- `contact_` untuk Contact page
- `home_` untuk Homepage
- `global_` untuk wording yang dipakai di banyak tempat

### 2. Grouping Fields
Gunakan `'group'` untuk mengelompokkan field di admin:
```php
'group' => 'Hero Section',
'group' => 'Content',
'group' => 'Footer',
```

### 3. Selalu Escape Output
```php
// Text
echo esc_html($value);

// URL
echo esc_url($value);

// HTML attribute
echo esc_attr($value);

// HTML content (WYSIWYG)
echo wp_kses_post($value);
```

### 4. Conditional Display
```php
<?php if (w_checked('about_show_team')) : ?>
    <!-- Team section -->
<?php endif; ?>

<?php if (!empty($w['hero']['background'])) : ?>
    <div style="background-image: url('<?php echo esc_url($w['hero']['background']); ?>')">
<?php endif; ?>
```

### 5. Default Values
Selalu sediakan default value untuk field penting:
```php
'about_cta_text' => [
    'label'   => 'CTA Text',
    'type'    => 'text',
    'default' => 'Get Started',  // Fallback jika admin belum mengisi
    'group'   => 'CTA',
],
```

### 6. Kombinasi dengan Global Wording
```php
// Gunakan global wording untuk text umum
$global = wording_global();

<button><?php echo esc_html($global['form']['submit']); ?></button>
<a href="#"><?php echo esc_html($global['text']['read_more']); ?></a>
```

---

## Troubleshooting

### Field tidak muncul di Admin
- Pastikan function name mengikuti pattern: `wording_page_{name}_fields()`
- Clear cache browser dan refresh halaman admin

### Value tidak tampil di frontend
- Pastikan sudah save di admin panel
- Check apakah key field sudah benar
- Gunakan `var_dump(w('field_key'))` untuk debug

### WYSIWYG tidak tampil sebagai visual editor
- WYSIWYG yang ditambahkan via JavaScript akan tampil sebagai textarea
- Save dan reload halaman untuk mendapatkan visual editor
