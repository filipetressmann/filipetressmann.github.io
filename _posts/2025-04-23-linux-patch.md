---
layout: post
title: Primeira contribuição ao Linux
comments: true
mathjax: true
author: Filipe Tressmann
---

Como parte da disciplina MAC0470, tive a oportunidade de enviar um patch para o kernel do Linux.

O patch foi uma contribuição ao driver `ti-ads124s08`. Esse driver, anteriormente, não fazia uso da função `iio_device_claim_direct`, responsável por garantir que diferentes modos de leitura não entrem em conflito entre si.

Para realizar essa modificação, precisei me aprofundar no funcionamento do subsistema IIO (Industrial I/O) do kernel, compreendendo tanto o propósito da função quanto a forma como ela interage com o restante do código. Também estudei como drivers são integrados ao sistema operacional, o que me deu uma visão mais ampla da arquitetura do Linux.

Além de adicionar a chamada à função mencionada, aproveitei para refatorar uma parte do código que estava duplicada. O trecho a seguir aparecia tanto na função `read_raw` quanto no `trigger handler`:

```c
ret = ads124s_write_reg(indio_dev, ADS124S08_INPUT_MUX, chan->channel);
if (ret) {
    dev_err(&priv->spi->dev, "Set ADC CH failed\n");
    goto out;
}

ret = ads124s_write_cmd(indio_dev, ADS124S08_START_CONV);
if (ret) {
    dev_err(&priv->spi->dev, "Start conversions failed\n");
    goto out;
}

ret = ads124s_read(indio_dev);
if (ret < 0) {
    dev_err(&priv->spi->dev, "Read ADC failed\n");
    goto out;
}

*val = ret;

ret = ads124s_write_cmd(indio_dev, ADS124S08_STOP_CONV);
if (ret) {
    dev_err(&priv->spi->dev, "Stop conversions failed\n");
    goto out;
}
```

Essa lógica foi extraída para uma função auxiliar, tornando o código mais limpo e reutilizável:

```c
static int  ads124s_read_channel(struct iio_dev *indio_dev, struct iio_chan_spec const *chan, int *val) {
       int ret = ads124s_write_reg(indio_dev, ADS124S08_INPUT_MUX, chan);
       if (ret) {
               dev_err(&priv->spi->dev, "Set ADC CH failed\n");
               return ret;
       }
       
       ret = ads124s_write_cmd(indio_dev, ADS124S08_START_CONV);
       if (ret) {
               dev_err(&priv->spi->dev, "Start ADC conversions failed\n");
               return ret;
       }

       ret = ads124s_read(indio_dev);
       if (ret < 0) {
               dev_err(&priv->spi->dev, "Read ADC failed\n");
               return ret;
       }

       *val = ret;

       ret = ads124s_write_cmd(indio_dev, ADS124S08_STOP_CONV);
       if (ret) {
               dev_err(&priv->spi->dev, "Stop conversions failed\n");
               return ret;
       }

       return IIO_VAL_INT;
}
```