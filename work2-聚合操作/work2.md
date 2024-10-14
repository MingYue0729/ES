**学院：省级示范性软件学院**

**课程：高级数据库技术与应用**

**题目：**《实验三：聚合操作练习》

**姓名：** 翁未未

**学号：** 2200770273

**班级：** 软工2202

**日期：** 2024-10-12

**实验环境：** Elasticsearch8.12.2和Kibana8.12.2

## 一、实验目的

1. **掌握Elasticsearch聚合查询**：通过实际操作，学习并掌握Elasticsearch聚合查询的编写和执行，这是处理和分析大规模数据集的关键技能。
2. **数据分析技能提升**：通过对电商数据的聚合分析，提升对数据的洞察力和分析能力，学习如何从大量数据中提取有价值的信息。
3. **实践数据库操作**：通过创建索引、导入数据和执行聚合查询，实践数据库的基本操作，加深对数据库管理系统的理解。
4. **解决实际问题**：应用Elasticsearch聚合功能解决具体的数据分析问题，如销售分析、客户行为分析等，增强将理论知识应用于实际问题解决的能力。

## 二、实验内容

**1. 统计每个产品类别的总销售额。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "total_sales_per_category": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "total_sales_per_category": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Jewelry",
          "doc_count": 14,
          "total_sales": {
            "value": 17477.37028503418
          }
        },
        {
          "key": "Electronics",
          "doc_count": 13,
          "total_sales": {
            "value": 18941.559997558594
          }
        },
        {
          "key": "Home Appliances",
          "doc_count": 13,
          "total_sales": {
            "value": 25865.190231323242
          }
        },
        {
          "key": "Beauty",
          "doc_count": 11,
          "total_sales": {
            "value": 22708.94012451172
          }
        },
        {
          "key": "Furniture",
          "doc_count": 10,
          "total_sales": {
            "value": 17228.31996154785
          }
        },
        {
          "key": "Books",
          "doc_count": 9,
          "total_sales": {
            "value": 11878.820098876953
          }
        },
        {
          "key": "Groceries",
          "doc_count": 9,
          "total_sales": {
            "value": 16172.980072021484
          }
        },
        {
          "key": "Fashion",
          "doc_count": 8,
          "total_sales": {
            "value": 7073.110048294067
          }
        },
        {
          "key": "Toys",
          "doc_count": 7,
          "total_sales": {
            "value": 10528.830047607422
          }
        },
        {
          "key": "Sports",
          "doc_count": 6,
          "total_sales": {
            "value": 13250.500122070312
          }
        }
      ]
    }
  }
}
```

**2. 计算每个城的平均订单金额。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "average_order_amount_per_city": {
      "terms": {
        "field": "customer_city"
      },
      "aggs": {
        "average_order_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "average_order_amount_per_city": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Los Angeles",
          "doc_count": 13,
          "average_order_amount": {
            "value": 1482.7977142333984
          }
        },
        {
          "key": "San Diego",
          "doc_count": 13,
          "average_order_amount": {
            "value": 1878.4761915940505
          }
        },
        {
          "key": "San Jose",
          "doc_count": 12,
          "average_order_amount": {
            "value": 1587.2216771443684
          }
        },
        {
          "key": "Philadelphia",
          "doc_count": 11,
          "average_order_amount": {
            "value": 1370.19365345348
          }
        },
        {
          "key": "New York",
          "doc_count": 10,
          "average_order_amount": {
            "value": 1801.9510040283203
          }
        },
        {
          "key": "Chicago",
          "doc_count": 9,
          "average_order_amount": {
            "value": 1062.1610989040798
          }
        },
        {
          "key": "Houston",
          "doc_count": 9,
          "average_order_amount": {
            "value": 1735.199978298611
          }
        },
        {
          "key": "Dallas",
          "doc_count": 8,
          "average_order_amount": {
            "value": 1291.8024978637695
          }
        },
        {
          "key": "Phoenix",
          "doc_count": 8,
          "average_order_amount": {
            "value": 2315.7625274658203
          }
        },
        {
          "key": "San Antonio",
          "doc_count": 7,
          "average_order_amount": {
            "value": 1607.7128516605922
          }
        }
      ]
    }
  }
}
```

**3. 找出销量最高的前5个产品。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top_products": {
      "terms": {
        "field": "product_name",
        "size": 5,
        "order": {
          "total_quantity": "desc"
        }
      },
      "aggs": {
        "total_quantity": {
          "sum": {
            "field": "quantity"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_products": {
      "doc_count_error_upper_bound": -1,
      "sum_other_doc_count": 67,
      "buckets": [
        {
          "key": "Elite Accessory",
          "doc_count": 6,
          "total_quantity": {
            "value": 25
          }
        },
        {
          "key": "Ultra Device",
          "doc_count": 7,
          "total_quantity": {
            "value": 24
          }
        },
        {
          "key": "Ultra Accessory",
          "doc_count": 8,
          "total_quantity": {
            "value": 22
          }
        },
        {
          "key": "Ultra Gadget",
          "doc_count": 6,
          "total_quantity": {
            "value": 21
          }
        },
        {
          "key": "Ultra Tool",
          "doc_count": 6,
          "total_quantity": {
            "value": 21
          }
        }
      ]
    }
  }
}
```

**4.计算男性和女性客户的平均年龄。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "average_age_by_gender": {
      "terms": {
        "field": "customer_gender"
      },
      "aggs": {
        "average_age": {
          "avg": {
            "field": "customer_age"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "average_age_by_gender": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "male",
          "doc_count": 55,
          "average_age": {
            "value": 45.61818181818182
          }
        },
        {
          "key": "female",
          "doc_count": 45,
          "average_age": {
            "value": 43.888888888888886
          }
        }
      ]
    }
  }
}
```

**5. 统计每种支付方式的使用次数和总金额。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "payment_method_stats": {
      "terms": {
        "field": "payment_method"
      },
      "aggs": {
        "usage_count": {
          "value_count": {
            "field": "payment_method"
          }
        },
        "total_amount": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "payment_method_stats": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Cash on Delivery",
          "doc_count": 29,
          "usage_count": {
            "value": 29
          },
          "total_amount": {
            "value": 38746.55044555664
          }
        },
        {
          "key": "Debit Card",
          "doc_count": 29,
          "usage_count": {
            "value": 29
          },
          "total_amount": {
            "value": 48975.19040107727
          }
        },
        {
          "key": "Credit Card",
          "doc_count": 22,
          "usage_count": {
            "value": 22
          },
          "total_amount": {
            "value": 36358.470153808594
          }
        },
        {
          "key": "PayPal",
          "doc_count": 20,
          "usage_count": {
            "value": 20
          },
          "total_amount": {
            "value": 37045.40998840332
          }
        }
      ]
    }
  }
}
```

**6. 计算每月的总销售额。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "total_sales_per_month": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "month"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "total_sales_per_month": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 12,
          "total_sales": {
            "value": 24381.61993408203
          }
        },
        {
          "key_as_string": "2023-02-01T00:00:00.000Z",
          "key": 1675209600000,
          "doc_count": 11,
          "total_sales": {
            "value": 18870.850006103516
          }
        },
        {
          "key_as_string": "2023-03-01T00:00:00.000Z",
          "key": 1677628800000,
          "doc_count": 10,
          "total_sales": {
            "value": 17959.33026123047
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 9,
          "total_sales": {
            "value": 18775.959869384766
          }
        },
        {
          "key_as_string": "2023-05-01T00:00:00.000Z",
          "key": 1682899200000,
          "doc_count": 10,
          "total_sales": {
            "value": 11713.169967651367
          }
        },
        {
          "key_as_string": "2023-06-01T00:00:00.000Z",
          "key": 1685577600000,
          "doc_count": 9,
          "total_sales": {
            "value": 6771.1600341796875
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 7,
          "total_sales": {
            "value": 9110.010131835938
          }
        },
        {
          "key_as_string": "2023-08-01T00:00:00.000Z",
          "key": 1690848000000,
          "doc_count": 8,
          "total_sales": {
            "value": 12135.870210647583
          }
        },
        {
          "key_as_string": "2023-09-01T00:00:00.000Z",
          "key": 1693526400000,
          "doc_count": 3,
          "total_sales": {
            "value": 8615.500122070312
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 6,
          "total_sales": {
            "value": 12106.000183105469
          }
        },
        {
          "key_as_string": "2023-11-01T00:00:00.000Z",
          "key": 1698796800000,
          "doc_count": 6,
          "total_sales": {
            "value": 8176.529968261719
          }
        },
        {
          "key_as_string": "2023-12-01T00:00:00.000Z",
          "key": 1701388800000,
          "doc_count": 9,
          "total_sales": {
            "value": 12509.620300292969
          }
        }
      ]
    }
  }
}
```

**7. 找出平均订单金额最高的前3个客户。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top3_customers_average_order_amount": {
      "terms": {
        "field": "customer_id",
        "size": 3
      },
      "aggs": {
        "average_order_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top3_customers_average_order_amount": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 94,
      "buckets": [
        {
          "key": "C003",
          "doc_count": 2,
          "average_order_amount": {
            "value": 1896.160026550293
          }
        },
        {
          "key": "C336",
          "doc_count": 2,
          "average_order_amount": {
            "value": 896.1250152587891
          }
        },
        {
          "key": "C365",
          "doc_count": 2,
          "average_order_amount": {
            "value": 428.7350082397461
          }
        }
      ]
    }
  }
}
```

**8. 计算每个年龄段（18-30，31-50，51+）的客户数量。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "customer_age_groups": {
      "range": {
        "field": "customer_age",
        "ranges": [
          {"from": 18, "to": 30},
          {"from": 30, "to": 50},
          {"from": 50} 

​       ]
       }
     }
  }
}
```

运行结果：

```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "customer_age_groups": {
      "buckets": [
        {
          "key": "18.0-30.0",
          "from": 18,
          "to": 30,
          "doc_count": 24
        },
        {
          "key": "30.0-50.0",
          "from": 30,
          "to": 50,
          "doc_count": 32
        },
        {
          "key": "50.0-*",
          "from": 50,
          "doc_count": 44
        }
      ]
    }
  }
}
```

**9. 计算每个产品类别的平均单价。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "average_price_per_category": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "average_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "average_price_per_category": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Jewelry",
          "doc_count": 14,
          "average_price": {
            "value": 537.6807218279157
          }
        },
        {
          "key": "Electronics",
          "doc_count": 13,
          "average_price": {
            "value": 484.44461763822113
          }
        },
        {
          "key": "Home Appliances",
          "doc_count": 13,
          "average_price": {
            "value": 645.6961549612192
          }
        },
        {
          "key": "Beauty",
          "doc_count": 11,
          "average_price": {
            "value": 701.0781749378551
          }
        },
        {
          "key": "Furniture",
          "doc_count": 10,
          "average_price": {
            "value": 518.5519981384277
          }
        },
        {
          "key": "Books",
          "doc_count": 9,
          "average_price": {
            "value": 524.0977762010363
          }
        },
        {
          "key": "Groceries",
          "doc_count": 9,
          "average_price": {
            "value": 566.4644444783529
          }
        },
        {
          "key": "Fashion",
          "doc_count": 8,
          "average_price": {
            "value": 369.4774992465973
          }
        },
        {
          "key": "Toys",
          "doc_count": 7,
          "average_price": {
            "value": 485.97999899727955
          }
        },
        {
          "key": "Sports",
          "doc_count": 6,
          "average_price": {
            "value": 483.9566599527995
          }
        }
      ]
    }
  }
}
```

**10.找出订单数量最多的前5个城市。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top5_cities_order_count": {
      "terms": {
        "field": "customer_city",
        "size": 5
      },
      "aggs": {
        "order_count": {
          "value_count": {
            "field": "customer_city"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top5_cities_order_count": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 41,
      "buckets": [
        {
          "key": "Los Angeles",
          "doc_count": 13,
          "order_count": {
            "value": 13
          }
        },
        {
          "key": "San Diego",
          "doc_count": 13,
          "order_count": {
            "value": 13
          }
        },
        {
          "key": "San Jose",
          "doc_count": 12,
          "order_count": {
            "value": 12
          }
        },
        {
          "key": "Philadelphia",
          "doc_count": 11,
          "order_count": {
            "value": 11
          }
        },
        {
          "key": "New York",
          "doc_count": 10,
          "order_count": {
            "value": 10
          }
        }
      ]
    }
  }
}
```

**11. 计算每个季度的平均订单金额。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "average_order_per_quarter": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "quarter"
      },
      "aggs": {
        "average_order_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "average_order_per_quarter": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 33,
          "average_order_amount": {
            "value": 1854.9030364065459
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 28,
          "average_order_amount": {
            "value": 1330.724638257708
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 18,
          "average_order_amount": {
            "value": 1658.9655813641018
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 21,
          "average_order_amount": {
            "value": 1561.530973888579
          }
        }
      ]
    }
  }
}
```

**12. 统计每个产品类别中的商品数量。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "product_count_per_category": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "product_count": {
          "sum": {
            "field": "quantity"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "product_count_per_category": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "Jewelry",
          "doc_count": 14,
          "product_count": {
            "value": 31
          }
        },
        {
          "key": "Electronics",
          "doc_count": 13,
          "product_count": {
            "value": 39
          }
        },
        {
          "key": "Home Appliances",
          "doc_count": 13,
          "product_count": {
            "value": 40
          }
        },
        {
          "key": "Beauty",
          "doc_count": 11,
          "product_count": {
            "value": 31
          }
        },
        {
          "key": "Furniture",
          "doc_count": 10,
          "product_count": {
            "value": 32
          }
        },
        {
          "key": "Books",
          "doc_count": 9,
          "product_count": {
            "value": 23
          }
        },
        {
          "key": "Groceries",
          "doc_count": 9,
          "product_count": {
            "value": 30
          }
        },
        {
          "key": "Fashion",
          "doc_count": 8,
          "product_count": {
            "value": 23
          }
        },
        {
          "key": "Toys",
          "doc_count": 7,
          "product_count": {
            "value": 22
          }
        },
        {
          "key": "Sports",
          "doc_count": 6,
          "product_count": {
            "value": 26
          }
        }
      ]
    }
  }
}
```

**13. 计算男性和女性客户的平均订单金额。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "average_order_amount_by_gender": {
      "terms": {
        "field": "customer_gender"
      },
      "aggs": {
        "average_order_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "average_order_amount_by_gender": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "male",
          "doc_count": 55,
          "average_order_amount": {
            "value": 1798.5043728915127
          }
        },
        {
          "key": "female",
          "doc_count": 45,
          "average_order_amount": {
            "value": 1382.397343995836
          }
        }
      ]
    }
  }
}
```

**14. 找出总销售额最高的前10个日期。**

```
GET /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "top_dates_by_total_sales": {
      "terms": {
        "field": "order_date",
        "size": 10,
        "order": {
          "total_sales": "desc"
        }
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "top_dates_by_total_sales": {
      "doc_count_error_upper_bound": -1,
      "sum_other_doc_count": 86,
      "buckets": [
        {
          "key": 1682726400000,
          "key_as_string": "2023-04-29T00:00:00.000Z",
          "doc_count": 2,
          "total_sales": {
            "value": 5951.77001953125
          }
        },
        {
          "key": 1677628800000,
          "key_as_string": "2023-03-01T00:00:00.000Z",
          "doc_count": 2,
          "total_sales": {
            "value": 5457.640075683594
          }
        },
        {
          "key": 1703980800000,
          "key_as_string": "2023-12-31T00:00:00.000Z",
          "doc_count": 2,
          "total_sales": {
            "value": 5177.3402099609375
          }
        },
        {
          "key": 1694736000000,
          "key_as_string": "2023-09-15T00:00:00.000Z",
          "doc_count": 1,
          "total_sales": {
            "value": 4886.10009765625
          }
        },
        {
          "key": 1696291200000,
          "key_as_string": "2023-10-03T00:00:00.000Z",
          "doc_count": 1,
          "total_sales": {
            "value": 4858.9501953125
          }
        },
        {
          "key": 1678665600000,
          "key_as_string": "2023-03-13T00:00:00.000Z",
          "doc_count": 1,
          "total_sales": {
            "value": 4592.9501953125
          }
        },
        {
          "key": 1691107200000,
          "key_as_string": "2023-08-04T00:00:00.000Z",
          "doc_count": 1,
          "total_sales": {
            "value": 4131.9501953125
          }
        },
        {
          "key": 1677283200000,
          "key_as_string": "2023-02-25T00:00:00.000Z",
          "doc_count": 2,
          "total_sales": {
            "value": 3872.6699829101562
          }
        },
        {
          "key": 1681171200000,
          "key_as_string": "2023-04-11T00:00:00.000Z",
          "doc_count": 1,
          "total_sales": {
            "value": 3790.89990234375
          }
        },
        {
          "key": 1674777600000,
          "key_as_string": "2023-01-27T00:00:00.000Z",
          "doc_count": 1,
          "total_sales": {
            "value": 3714.25
          }
        }
      ]
    }
  }
}
```

**15. 计算每个季度销售额最高的产品类别。**

```
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "product_category_by_quarter": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "quarter"
      },
      "aggs": {
        "top_categories": {
          "terms": {
            "field": "product_category",
            "size": 1
          },
          "aggs": {
            "total_sales": {
              "sum": {
                "field": "total_amount"
              }
            }
          }
        }
      }
    }
  }
}
```

运行结果：

```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 100,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "product_category_by_quarter": {
      "buckets": [
        {
          "key_as_string": "2023-01-01T00:00:00.000Z",
          "key": 1672531200000,
          "doc_count": 33,
          "top_categories": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 25,
            "buckets": [
              {
                "key": "Jewelry",
                "doc_count": 8,
                "total_sales": {
                  "value": 11757.280212402344
                }
              }
            ]
          }
        },
        {
          "key_as_string": "2023-04-01T00:00:00.000Z",
          "key": 1680307200000,
          "doc_count": 28,
          "top_categories": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 24,
            "buckets": [
              {
                "key": "Furniture",
                "doc_count": 4,
                "total_sales": {
                  "value": 5408.469985961914
                }
              }
            ]
          }
        },
        {
          "key_as_string": "2023-07-01T00:00:00.000Z",
          "key": 1688169600000,
          "doc_count": 18,
          "top_categories": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 15,
            "buckets": [
              {
                "key": "Electronics",
                "doc_count": 3,
                "total_sales": {
                  "value": 3483.2700805664062
                }
              }
            ]
          }
        },
        {
          "key_as_string": "2023-10-01T00:00:00.000Z",
          "key": 1696118400000,
          "doc_count": 21,
          "top_categories": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 17,
            "buckets": [
              {
                "key": "Beauty",
                "doc_count": 4,
                "total_sales": {
                  "value": 9416.89013671875
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

## 三、问题及解决办法

#### 问题1：Elasticsearch拒绝执行聚合查询

**具体表现**：执行聚合查询时，Elasticsearch返回错误，指出查询语句有误。 

**解决办法**：检查查询语句的语法是否正确，特别是聚合部分的格式。

#### 问题2：查询结果中缺少某些字段

**具体表现**：在执行聚合查询后，发现返回的结果中缺少某些预期的字段。 

**解决办法**：重新检查索引的映射，确认所有需要的字段都已正确索引，如果字段存在但未出现在结果中，确保查询中包含了这些字段的聚合。
