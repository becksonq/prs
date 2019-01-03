<?php

namespace console\controllers;

use Yii;
use yii\console\Controller;
use GuzzleHttp\Client;

class PrsController extends Controller
{
    /**
     * @var string
     */
    private $_sourceUrl = 'https://ru.investing.com/currencies/eur-usd';

    /**
     * @var int
     */
    private $_highValue = 1450;

    /**
     * @var int
     */
    private $_lowValue = 1250;

    /**
     * @return int
     * @throws \GuzzleHttp\Exception\GuzzleException
     */
    public function actionIndex()
    {
        $document = $this->_getBody($this->_sourceUrl);
        $info = $document->find('#last_last')->text();

        if ((int)$info > $this->_highValue) {
            $this->_sendMail($info);
        }

        if ((int)$info < $this->_lowValue) {
            $this->_sendMail($info);
        }

        return 0;
    }

    /**
     * @param $text
     */
    private function _sendMail($text)
    {
        if (!Yii::$app->mailer->compose()
            ->setTo(Yii::$app->params['managerEmail'])
            ->setFrom(Yii::$app->params['adminEmail'])
            ->setSubject('Receive Info')
            ->setTextBody($text)
            ->send()) {

            $this->stdout("Cant't send mail" . PHP_EOL);
        }
    }

    /**
     * @param $url
     * @return \phpQueryObject|\QueryTemplatesParse|\QueryTemplatesSource|\QueryTemplatesSourceQuery
     * @throws \GuzzleHttp\Exception\GuzzleException
     */
    private function _getBody($url)
    {
        $client = new Client();
        $res = $client->request('GET', $url);
        $body = $res->getBody();
        $document = \phpQuery::newDocumentHTML($body);

        return $document;
    }
}
