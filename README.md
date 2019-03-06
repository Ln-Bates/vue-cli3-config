const
        baseSrc = 'src/pages/',
        fs = require('fs'),
        path = require('path'),
        dirs = fs.readdirSync('./src/pages'); // 读取 src下面的文件夹名称

let pages = {};

function resolve(dir) {
    return path.join(__dirname, dir)
}

dirs.forEach((dir) => {
    pages[dir] = `${baseSrc}${dir}/${dir}.js`;
});

module.exports = {
    pages, // 多页面
    publicPath: './', // 资源引入路径
    outputDir: '', // 输出路径 默认为dist
    productionSourceMap: false, // 是否需要生产环境的sourceMap文件
    chainWebpack: (config) => { // 别名
        config.resolve.alias
                .set('@$', resolve('src'))
                .set('assets', resolve('src/assets'))
                .set('css', resolve('src/assets/css/private'))
                .set('img', resolve('src/assets/img/private'))
                .set('js', resolve('src/assets/js'))
                .set('components', resolve('src/components'))
                .set('pages', resolve('src/pages'));
    },
    devServer: {
        proxy: {
            '/api': {
                target: '',// 代理服务器域名
                changeOrigin: true,
                pathRewrite: {
                    '^/api': ''
                }
            }
        }
    },
    css: {
        loaderOptions: {
            postcss: {
                plugins: [require('postcss-px2rem')({ // px转rem
                    remUnit: 75
                })]
            }
        }
    }
};
