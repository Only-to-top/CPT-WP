# new-post_type-WP

<h2>Создание нового типа записи а также таксономии</h2>

```php
// New post type - Услуги
add_action( 'init', 'register_post_type_services' );
function register_post_type_services(){
    register_post_type('services', array(
        'label'  => null,
        'labels' => [
            'name'               => 'Услуги', // основное название для типа записи
            'singular_name'      => 'Услуга', // название для одной записи этого типа
            'add_new'            => 'Добавить', // для добавления новой записи
            'add_new_item'       => 'Добавление', // заголовка у вновь создаваемой записи в админ-панели.
            'edit_item'          => 'Редактирование', // для редактирования типа записи
            'new_item'           => 'Новая', // текст новой записи
            'view_item'          => 'Смотреть', // для просмотра записи этого типа (ед. число).
            'search_items'       => 'Искать', // для поиска по этим типам записи
            'not_found'          => 'Не найдено', // если в результате поиска ничего не было найдено
            'not_found_in_trash' => 'Не найдено в корзине', // если не было найдено в корзине
            'menu_name'          => 'Услуги', // название меню
        ],
        'description'            => '',
        'public'                 => true,
        // 'exclude_from_search' => true, // будет исключен из запросов типа query_posts()
        // 'capability_type'     => 'page', // default 'post'
        // 'hierarchical'        => true, // будут ли записи этого типа иметь древовидную структуру (как постоянные страницы), default 'false'
        'menu_icon'              => 'dashicons-clipboard',
        'supports'               => [
            'title',
            'editor',
            // 'excerpt',
            // 'trackbacks',
            // 'custom-fields',
            // 'comments',
            // 'revisions',
            // 'thumbnail',
            // 'author',
            // 'page-attributes', # атрибуты страницы
        ],
        'has_archive'           => false, // исключить из sitemap
        'rewrite'               => true,
        'query_var'             => true,
    ) );
}
```

## С категориями

```php
add_action('init', 'register_post_production');
function register_post_production()
{
    register_taxonomy('production_categories', ['production'], [
        'label' => 'Категории',
        'labels' => [
            'name' => 'Категории',
            'singular_name' => 'Категория',
            'search_items' => 'Искать',
            'all_items' => 'Все',
            'view_item ' => 'Смотреть',
            'edit_item' => 'Редактировать',
            'update_item' => 'Обновить',
            'add_new_item' => 'Добавить',
            'new_item_name' => 'Новое',
            'menu_name' => 'Категории',
        ],
        'description' => '',
        'public' => true,
        'show_ui' => true,
        'show_admin_column' => 'true',
        'show_in_quick_edit' => 'true',
        'hierarchical' => true,
        'rewrite' => array('slug' => 'production'),
    ]);
    register_post_type('production', array(
        'label' => null,
        'labels' => array(
            'name' => 'Продукция',
            'singular_name' => 'Продукция',
            'add_new' => 'Добавить',
            'add_new_item' => 'Добавление',
            'edit_item' => 'Редактирование',
            'new_item' => 'Новая',
            'view_item' => 'Смотреть',
            'search_items' => 'Искать',
            'not_found' => 'Не найдено',
            'not_found_in_trash' => 'Не найдено в корзине',
            'parent_item_colon' => '',
            'menu_name' => 'Продукция',
        ),
        'description' => '',
        'public' => true,
        'show_ui' => true,
        'show_in_nav_menus' => true,
        'show_in_menu' => true,
        'show_in_admin_bar' => true,
        'menu_icon' => 'dashicons-cart',
        'hierarchical' => false,
        'supports'               => [
            'title',
            // 'editor',
            // 'excerpt',
            // 'trackbacks',
            // 'custom-fields',
            // 'comments',
            // 'revisions',
            'thumbnail',
            // 'author',
            // 'page-attributes', # атрибуты страницы
        ],
        'has_archive' => 'production',
        'rewrite' => array('slug' => 'production/%production_categories%', 'with_front' => false, 'pages' => true, 'feeds' => false, 'feed' => false),
    ));
}

add_filter('post_type_link', 'production_permalink', 1, 2);
function production_permalink($permalink, $post)
{
    if (strpos($permalink, '%production_categories%') === FALSE)
        return $permalink;
    $terms = get_the_terms($post, 'production_categories');
    if (!is_wp_error($terms) && !empty($terms) && is_object($terms[0]))
        $taxonomy_slug = $terms[0]->slug;
    else
        $taxonomy_slug = 'no_production_categories';
    return str_replace('%production_categories%', $taxonomy_slug, $permalink);
}

# fix taxonomy pagination
add_filter('pre_get_posts', 'tax_city_posts_per_page');
function tax_city_posts_per_page($query)
{
    if (is_tax('production_categories')) {
        $query->set('posts_per_page', 1);
    }
    return $query;
}
```

### С категориями - услуги

```php
<?php

add_action('init', 'register_post_service');
function register_post_service()
{
    register_taxonomy('services_categories', ['services'], [
        'label' => 'Категории',
        'labels' => [
            'name' => 'Категории',
            'singular_name' => 'Категория',
            'search_items' => 'Искать',
            'all_items' => 'Все',
            'view_item ' => 'Смотреть',
            'edit_item' => 'Редактировать',
            'update_item' => 'Обновить',
            'add_new_item' => 'Добавить',
            'new_item_name' => 'Новое',
            'menu_name' => 'Категории',
        ],
        'description' => '',
        'public' => true,
        'show_ui' => true,
        'show_admin_column' => 'true',
        'show_in_quick_edit' => 'true',
        'hierarchical' => true,
        'rewrite' => array('slug' => 'services'),
    ]);
    register_post_type('services', array(
        'label' => null,
        'labels' => array(
            'name' => 'Услуги',
            'singular_name' => 'Услуга',
            'add_new' => 'Добавить',
            'add_new_item' => 'Добавление',
            'edit_item' => 'Редактирование',
            'new_item' => 'Новая',
            'view_item' => 'Смотреть',
            'search_items' => 'Искать',
            'not_found' => 'Не найдено',
            'not_found_in_trash' => 'Не найдено в корзине',
            'parent_item_colon' => '',
            'menu_name' => 'Услуги',
        ),
        'description' => '',
        'public' => true,
        'show_ui' => true,
        'show_in_nav_menus' => true,
        'show_in_menu' => true,
        'show_in_admin_bar' => true,
        'menu_icon' => 'dashicons-cart',
        'hierarchical' => false,
        'supports'               => [
            'title',
            // 'editor',
            // 'excerpt',
            // 'trackbacks',
            // 'custom-fields',
            // 'comments',
            // 'revisions',
            'thumbnail',
            // 'author',
            // 'page-attributes', # атрибуты страницы
        ],
        'has_archive' => 'services',
        'rewrite' => array('slug' => 'services/%services_categories%', 'with_front' => false, 'pages' => true, 'feeds' => false, 'feed' => false),
    ));
}

add_filter('post_type_link', 'services_permalink', 1, 2);
function services_permalink($permalink, $post)
{
    if (strpos($permalink, '%services_categories%') === FALSE)
        return $permalink;
    $terms = get_the_terms($post, 'services_categories');
    if (!is_wp_error($terms) && !empty($terms) && is_object($terms[0]))
        $taxonomy_slug = $terms[0]->slug;
    else
        $taxonomy_slug = 'no_services_categories';
    return str_replace('%services_categories%', $taxonomy_slug, $permalink);
}
```
