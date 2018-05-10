---
title: 将typecho转文章转换到hexo
date: 2017-02-02 23:56
categories:
tags:
  - 树莓派
  - typecho
  - hexo
---

由于树莓派重做了好几次系统，好久没打理blog了，
今天整理下，发现原来在树莓派上搭建的typecho后台管理进不去了，一番折腾也没好用。干脆转到静态网页吧。与就将原 `typecho` 的blog转到 `hexo`

```php

<?php

$db = new SQLite3('typecho.db');

$result = $db->query("SELECT a.cid, a.title, a.slug, a.text, a.created, a.status,
    CASE c.type
        WHEN 'tag' THEN group_concat(c.name)
    END tags,
    CASE c.type
        WHEN 'category' THEN group_concat(c.name)
    END categories
    FROM typecho_contents AS a
    LEFT JOIN typecho_relationships AS b ON b.cid=a.cid
    LEFT JOIN typecho_metas AS c ON c.mid=b.mid
    GROUP BY a.cid
    ORDER BY a.cid DESC"
);

while ($row = $result->fetchArray(SQLITE3_ASSOC)) {
    generate($row);
}

function generate($data)
{
    $foler = __DIR__ . '/source/_drafts/';

    $tpl = file_get_contents(__DIR__ . '/scaffolds/post.md');

    $filename = $data['slug'] . '.md';
    if ($data['status'] == 'publish') {
        $foler = __DIR__ . '/source/_posts/';
    }

    $tpl = str_replace('{{ title }}', $data['title'], $tpl);
    $tpl = str_replace('{{ date }}', date('Y-m-d H:i:s', $data['created']), $tpl);
    $tpl = str_replace('{{ tags }}', $data['tags'] ? '[' . $data['tags'] . ']' : '', $tpl);
    $tpl = str_replace('{{ categories }}', $data['categories'] ? '[' . $data['categories'] . ']' : '', $tpl);

    $tpl .= $data['text'];
    file_put_contents($foler . $filename, $tpl);
}
```

https://gist.github.com/congpeijun/bdf308eec07458d2c7f57f4922458559