# Xhprof

```php
<?php

namespace app\controller;

use app\BaseController;

class Index extends BaseController
{
    public function index()
    {
        // 开启
        xhprof_enable(XHPROF_FLAGS_NO_BUILTINS | XHPROF_FLAGS_CPU | XHPROF_FLAGS_MEMORY);

        // 业务代码
        for($i = 0; $i < 100; $i++) {
            cache('test', 'test');
        }

        // 保存 xhprof 数据
        $xhprof_data = xhprof_disable();

        // 引入 xhprof，可以使用 ghcr.io/hongfs/env:php82-cli-debug
        include_once "/xhprof_code/xhprof_lib/utils/xhprof_lib.php";
        include_once "/xhprof_code/xhprof_lib/utils/xhprof_runs.php";

        $xhprof = new \XHProfRuns_Default();
        $xhprof->save_run($xhprof_data, 'hongfs');

        return json([]);
    }

    // xhprof 页面
    public function xhprof()
    {
        $run = input('run', '');
        $wts = '';
        $symbol = input('symbol', '');
        $sort = input('sort', 'wt');
        $run1 = '';
        $run2 = '';
        $source = input('source', 'hongfs');
        $all = (int) input('all', 1);
        // $xhprof_cdn = 'https://xxxx'; // 下载静态文件地址

        $_SERVER['SCRIPT_NAME'] = request()->baseUrl(true);

        include_once '/xhprof_code/xhprof_html/index.php';
    }

    // xhprof callgraph 页面
    public function callgraph()
    {
        $threshold = 0.01;
        $type = input('type', 'svg');
        $run1 = '';
        $run2 = '';
        $func = '';
        $critical = '';
        $run = input('run', '');
        $source = input('source', 'hongfs');

        include_once '/xhprof_code/xhprof_html/callgraph.php';
    }
}
```
