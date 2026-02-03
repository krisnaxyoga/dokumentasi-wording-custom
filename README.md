# Theme Wording System

Sistem manajemen teks/wording untuk theme dengan admin panel.

## Quick Start - Cara Penggunaan di Template

### 1. Text Biasa
```php
// Get value
$title = w('field_key');

// Echo (escaped)
we('field_key');

// Dengan default
we('field_key', 'Default Text');
```

### 2. URL
```php
<a href="<?php we_url('link_url'); ?>">Link</a>
```

### 3. WYSIWYG / HTML Content
```php
<div class="content">
    <?php we_html('wysiwyg_field'); ?>
</div>
```

### 4. Image
```php
// Single image field
<img src="<?php we_url('image_field'); ?>" alt="">

// Image dengan fallback (upload first, URL fallback)
<img src="<?php we_img('upload_field', 'url_field'); ?>" alt="">

// Atau langsung output tag
<?php w_img_tag('image_field', 'css-class', 'Alt text'); ?>
<?php w_img_tag_fallback('upload_field', 'url_field', 'css-class', 'Alt text'); ?>
```

### 5. Repeater
```php
<?php foreach (w_repeater('items') as $item): ?>
    <div class="item">
        <h3><?php echo esc_html($item['title']); ?></h3>
        <p><?php echo esc_html($item['description']); ?></p>

        <!-- Nested repeater -->
        <?php if (!empty($item['gallery'])): ?>
            <?php foreach ($item['gallery'] as $img): ?>
                <img src="<?php echo esc_url($img['image_url']); ?>"
                     alt="<?php echo esc_attr($img['caption']); ?>">
            <?php endforeach; ?>
        <?php endif; ?>
    </div>
<?php endforeach; ?>
```

### 6. Checkbox
```php
<?php if (w_checked('show_feature')): ?>
    <div class="feature">...</div>
<?php endif; ?>
```

---

## Daftar Shorthand Functions

| Function | Kegunaan | Contoh |
|----------|----------|--------|
| `w($key)` | Get value | `$val = w('title')` |
| `we($key)` | Echo text (escaped) | `we('title')` |
| `we_url($key)` | Echo URL (escaped) | `we_url('link')` |
| `we_html($key)` | Echo HTML (WYSIWYG) | `we_html('content')` |
| `w_repeater($key)` | Get repeater array | `foreach(w_repeater('items'))` |
| `w_img($up, $url)` | Get image URL with fallback | `w_img('upload', 'url')` |
| `we_img($up, $url)` | Echo image URL with fallback | `we_img('upload', 'url')` |
| `w_checked($key)` | Check checkbox (bool) | `if(w_checked('show'))` |
| `w_img_tag($key)` | Output img tag | `w_img_tag('img', 'class')` |

---

## Mendefinisikan Fields (wording-fields.php)

```php
'section_name' => [
    'title' => 'Section Title',
    'fields' => [
        // Text
        'field_text' => [
            'label' => 'Text Field',
            'type'  => 'text',
            'default' => 'Default value',
        ],

        // Textarea
        'field_textarea' => [
            'label' => 'Textarea',
            'type'  => 'textarea',
        ],

        // WYSIWYG
        'field_wysiwyg' => [
            'label' => 'Content',
            'type'  => 'wysiwyg',
        ],

        // URL
        'field_url' => [
            'label' => 'Link URL',
            'type'  => 'url',
        ],

        // Image (Media Library)
        'field_image' => [
            'label' => 'Image',
            'type'  => 'image',
        ],

        // Image URL (External)
        'field_image_url' => [
            'label' => 'Image URL',
            'type'  => 'image_url',
        ],

        // Number
        'field_number' => [
            'label' => 'Number',
            'type'  => 'number',
            'min'   => 0,
            'max'   => 100,
        ],

        // Select
        'field_select' => [
            'label'   => 'Select',
            'type'    => 'select',
            'choices' => [
                'opt1' => 'Option 1',
                'opt2' => 'Option 2',
            ],
        ],

        // Checkbox
        'field_checkbox' => [
            'label'          => 'Enable Feature',
            'type'           => 'checkbox',
            'checkbox_label' => 'Yes, enable this',
        ],

        // Simple Repeater
        'field_repeater_simple' => [
            'label' => 'Items',
            'type'  => 'repeater',
        ],

        // Complex Repeater
        'field_repeater_complex' => [
            'label'     => 'Slides',
            'type'      => 'repeater',
            'subfields' => [
                'title' => 'Title',
                'url'   => [
                    'label' => 'Link',
                    'type'  => 'url',
                ],
                'image' => [
                    'label' => 'Image',
                    'type'  => 'image',
                ],
                'content' => [
                    'label' => 'Content',
                    'type'  => 'wysiwyg',
                ],
                // Nested repeater
                'gallery' => [
                    'label'     => 'Gallery',
                    'type'      => 'repeater',
                    'subfields' => [
                        'img' => [
                            'label' => 'Image',
                            'type'  => 'image',
                        ],
                        'caption' => 'Caption',
                    ],
                ],
            ],
        ],
    ],
],
```

---

## Supported Field Types

| Type | Deskripsi |
|------|-----------|
| `text` | Input text satu baris |
| `textarea` | Input text multi-baris |
| `wysiwyg` | WordPress visual editor |
| `url` | Input URL |
| `image` | Pilih dari Media Library |
| `image_url` | Input URL gambar eksternal |
| `number` | Input angka (support min/max) |
| `select` | Dropdown pilihan |
| `checkbox` | Checkbox boolean |
| `repeater` | Multiple items (bisa nested) |
