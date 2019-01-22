
## 简介
aliyun官方sdk开发包，支持yii2

## 下载安装
* 下载
```bash
git clone https://github.com/mrh111/aliyunsdk.git
```

* 安装

进入　aliyunsdk　目录，执行

```bash
composer require "mrh/aliyunsdk:dev-master"
```

* 项目中的使用

在config/main.php配置文件中定义component配置信息, 以视频播放vod为例
```bash
'components' => [
  .....
  'aliyunVod' => [
      'class' => 'common\components\AliyunVideo',
      'accessKeyId' => '666666',
      'accessKeySecret' => '999999999999999999999'
    ],
  ....
]

```

在项目common\components目录下创建 AliyunVideo类

```bash
namespace common\components;
use yii;
use yii\base\Component;
use DefaultAcsClient;
use DefaultProfile;

use vod\Request\V20170321 as Vod;

class AliyunVideo extends Component {

    public $accessKeyId = '';
    
    public $accessKeySecret = '';
    
    private $regionId = 'cn-shanghai';

    /**
     * 初始化
     * @return DefaultAcsClient
     */
    public function client(){
        $profile = DefaultProfile::getProfile($this->regionId, $this->accessKeyId, $this->accessKeySecret);
        return new DefaultAcsClient($profile);
    }

    /**
     * 获取播放凭证
     * @param $client
     * @param $videoid 视频ID
     * @return mixed
     */
    public function get_video_play_auth($client, $videoid){
        $request = new Vod\GetVideoPlayAuthRequest();
        $request->setAcceptFormat('JSON');
        $request->setRegionId($this->regionId);
        $request->setVideoId($videoid);
        $response = $client->getAcsResponse($request);
        return $response;
    }

    /**
     * 获取播放url地址信息
     * @param $client
     * @param $videoId
     * @return mixed
     */
    public function get_play_info($client, $videoId) {
        $request = new Vod\GetPlayInfoRequest();
        $request->setVideoId($videoId);
        $request->setAuthTimeout(3600*24);    // 播放地址过期时间（只有开启了URL鉴权才生效），默认为3600秒，支持设置最小值为3600秒
        $request->setAcceptFormat('JSON');
        return $client->getAcsResponse($request);
    }
    
}

```
在方法中调用

```bash
$video = yii::$app->aliyunVod;
$client = $video->client();
$info = $video->get_play_info($client, $videoId);
```
