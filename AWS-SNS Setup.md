# Steps to integrate AWS SNS Service with PHP Application.

## Install AWS SDK on codeigniter using composer use the commands [more info](https://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/installation.html).
 ```
 curl -sS https://getcomposer.org/installer | php
 ```
 ```
 php composer.phar require aws/aws-sdk-php
 ```
```
require 'vendor/autoload.php';
```

## Configure AWS SDK

```
use Aws\Sns\SnsClient;

$client = SnsClient::factory(array(
    'profile' => '<profile in your aws credentials file>',
    'region'  => '<region name>'
));
```

```
use Aws\Sns\SnsClient; //here you define what module(s) you want to use

class Aws_sdk {

    public $snsClient;
    public $ci;

    public function __construct() {
        $this->ci = & get_instance();
        $this->ci->load->config('aws_sdk');
        $this->snsClient = SnsClient::factory(array(
                    'key' => $this->ci->config->item('aws_access_key'),
                    'secret' => $this->ci->config->item('aws_secret_key'),
                    'region' => "us-west-2"
        )); //Change this to instantiate the module you want. Look at the documentation to find out what parameters you need. 
    }

    public function __call($name, $arguments = null) {
        if (!property_exists($this, $name)) {
            return call_user_func_array(array($this->snsClient, $name), $arguments);
        } //Change this accordingly too
    }

//Write your methods to call AWS functionality
    public function SendPushNotification($message, $target)
    {
        $result = $this->snsClient->publish(array(
            'TargetArn' => $target,
            'Message' => $message
        ));
    }
}
```

## SNS client is used to interact with the Amazon Simple Notification Service (Amazon SNS).

```
Aws\AwsClient implements Aws\AwsClientInterface uses Aws\AwsClientTrait
Extended by Aws\Sns\SnsClient
Namespace: Aws\Sns
Located at Sns/SnsClient.php
```

## API for AWS SNS

Use the API to create and manage the requests.

[AWS API](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#createtopic)

## Code to Send SMS

```
use Aws\Sns\SnsClient;

$sns = \Aws\Sns\SnsClient::factory(array(
    'credentials' => [
        'key'    => 'use Aws\Sns\SnsClient;

$sns = \Aws\Sns\SnsClient::factory(array(
    'credentials' => [
        'key'    => '<Key>',
        'secret' => '<Secret>',
    ],
    'region' => 'us-west-2',
    'version'  => 'latest',
));

$result = $sns->publish([
    'Message' => '<message>', // REQUIRED
    'MessageAttributes' => [
        'AWS.SNS.SMS.SenderID' => [
            'DataType' => 'String', // REQUIRED
            'StringValue' => '<sender_id>'
        ],
        'AWS.SNS.SMS.SMSType' => [
            'DataType' => 'String', // REQUIRED
            'StringValue' => 'Transactional' // or 'Promotional'
        ]
    ],
    'PhoneNumber' => '<phone_number>',
]);
print_r($result);
```