# Чтобы пагинация работала на страницах 2, 3, ... Слаг страницы не должен совпадать с post_type

```php
add_filter('redirect_canonical', 'custom_disable_redirect_canonical');
function custom_disable_redirect_canonical($redirect_url)
{
    if (is_singular('your_custom_post_type')) $redirect_url = false;
    return $redirect_url;
}
```

## Создание нового типа записи а также таксономии</h2>

```php
// Услуга
add_action('init', 'register_cpt_service');
function register_cpt_service() {
    register_post_type('service', array(
        'label'  => null,
        'labels' => [
            'name'               => 'Услуги', // основное название для типа записи
            'singular_name'      => 'Услуга', // название для одной записи этого типа
            'add_new'            => 'Добавить',
            'add_new_item'       => 'Добавление',
            'edit_item'          => 'Редактирование',
            'new_item'           => 'Новая',
            'view_item'          => 'Смотреть',
            'search_items'       => 'Искать',
            'not_found'          => 'Не найдено',
            'not_found_in_trash' => 'Не найдено в корзине',
            'menu_name'          => 'Услуги',
        ],
        'description'            => '',
        'public'                 => true, // false - exclude
        'has_archive'            => true, // /post_type/post_name
        // 'capability_type'     => 'page', // default 'post'
        // 'hierarchical'        => true, // будут ли записи этого типа иметь древовидную структуру (как постоянные страницы), default 'false'
        'menu_icon'              => 'dashicons-clipboard',
        'menu_position'          => 4,
        // 'show_in_rest'        => true, // true - использовать gutenberg, default - false
        'supports'               => [
            'title',
            'editor',
            // 'excerpt',
            // 'thumbnail',
            // 'author',
            // 'page-attributes', # атрибуты страницы
        ],
        'rewrite'               => true,
        // 'rewrite'               => array('slug'=>'services', 'hierarchical'=>false, 'with_front'=>false, 'feed'=>false ),
        'query_var'             => true,
    ) );
}
```

## С категориями

```php
add_action('init', 'register_cpt_production');
function register_cpt_production()
{
    register_taxonomy('production_category', ['production'], [
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
        'public' => true,
        'show_ui' => true,
        'show_in_nav_menus' => true,
        'show_in_menu' => true,
        'show_in_admin_bar' => true,
        'menu_icon' => 'dashicons-cart',
        'hierarchical' => false,
        'supports' => [
            'title',
            // 'editor',
            // 'excerpt',
            'thumbnail',
            // 'page-attributes', # атрибуты страницы
        ],
        'has_archive'            => true, # '/post_type/post_name'
        'has_archive' => 'production',
        'rewrite' => array('slug' => 'production/%production_category%', 'with_front' => false, 'pages' => true, 'feeds' => false, 'feed' => false),
    ));
}

add_filter('post_type_link', 'production_permalink', 1, 2);
function production_permalink($permalink, $post)
{
    if (strpos($permalink, '%production_category%') === FALSE) return $permalink;
    $terms = get_the_terms($post, 'production_category');
    if (!is_wp_error($terms) && !empty($terms) && is_object($terms[0])) $taxonomy_slug = $terms[0]->slug;
    else $taxonomy_slug = 'no_production_category';
    return str_replace('%production_category%', $taxonomy_slug, $permalink);
}

# fix taxonomy pagination
add_filter('pre_get_posts', 'tax_city_posts_per_page');
function tax_city_posts_per_page($query)
{
    if (is_tax('production_category')) {
        $query->set('posts_per_page', 1);
    }
    return $query;
}
```

### С категориями - услуги

```php
add_action('init', 'register_cpt_service');
function register_cpt_service()
{
    register_taxonomy('services_category', ['services'], [
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
        'rewrite' => array('slug' => 'services/%services_category%', 'with_front' => false, 'pages' => false, 'feeds' => false, 'feed' => false),
    ));
}

add_filter('post_type_link', 'services_permalink', 1, 2);
function services_permalink($permalink, $post)
{
    if (strpos($permalink, '%services_category%') === FALSE)
        return $permalink;
    $terms = get_the_terms($post, 'services_category');
    if (!is_wp_error($terms) && !empty($terms) && is_object($terms[0]))
        $taxonomy_slug = $terms[0]->slug;
    else
        $taxonomy_slug = 'no_services_category';
    return str_replace('%services_category%', $taxonomy_slug, $permalink);
}
```

### Exclude a post type (Yoast Seo)

```php
# Exclude a post type (Yoast Seo)
function sitemap_exclude_post_type_1($excluded, $post_type)
{
    return $post_type === 'team';
}
add_filter('wpseo_sitemap_exclude_post_type', 'sitemap_exclude_post_type_1', 10, 2);
```

### Тип записи без страницы (ссылки)

```php
// exclude page url
'public'                 => false,
'query_var'              => false,
'publicly_queryable'     => false,
'show_ui'                => true,
'exclude_from_search'    => true,
'show_in_nav_menus'      => false,
'has_archive'            => false,
'rewrite'                => false,
```
