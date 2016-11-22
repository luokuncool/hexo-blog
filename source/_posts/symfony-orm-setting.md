---
layout: post
title:  "Symfony ORM 相关配置"
date:   2016-11-21 22:32:34 +0800
tags:
  - PHP
  - Symfony
---


1、给数据表添加索引
`@ORM\Table(indexes={@ORM\Index(name="email_address_idx", columns={"email_address"})})`  

2、给字段添加注释  
`@ORM\Column(name="createTime", type="integer", options={"comment":"创建时间"})`  

实例：

```php
<?php
namespace BackendBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;

/**
 * Category
 *
 * @ORM\Table(name="category", options={"comment":"分类表"}, indexes={@ORM\Index(name="pIdTargetEntity", columns={"pId","targetEntity"})})
 * @ORM\Entity(repositoryClass="BackendBundle\Repository\CategoryRepository")
 */
class Category
{
    /**
     * @var integer
     *
     * @ORM\Column(name="id", type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    private $id;

    /**
     * @var string
     *
     * @Assert\NotBlank(message="category.name.required")
     * @ORM\Column(name="name", type="string", length=255, options={"comment":"分类名称"})
     */
    private $name;

    /**
     * @var integer
     *
     * @ORM\Column(name="status", type="boolean", options={"comment":"状态：-1、删除；0、不显示；1、显示"})
     */
    private $status;

    /**
     * @var integer
     *
     * @ORM\Column(name="targetEntity", type="string", length=64, options={"comment":"关联数据表"})
     */
    private $targetEntity;

    /**
     * @var integer
     *
     * @ORM\Column(name="pId", type="smallint", options={"comment":"父类别ID"})
     */
    private $pId;


    /**
     * @var integer
     *
     * @ORM\Column(name="createTime", type="integer", options={"comment":"创建时间"})
     */
    private $createTime;


    /**
     * Get id
     *
     * @return integer
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * Set name
     *
     * @param string $name
     *
     * @return Category
     */
    public function setName($name)
    {
        $this->name = $name;

        return $this;
    }

    /**
     * Get name
     *
     * @return string
     */
    public function getName()
    {
        return $this->name;
    }

    /**
     * Set status
     *
     * @param integer $status
     *
     * @return Category
     */
    public function setStatus($status)
    {
        $this->status = $status;

        return $this;
    }

    /**
     * Get status
     *
     * @return integer
     */
    public function getStatus()
    {
        return $this->status;
    }

    /**
     * @return int
     */
    public function getTargetEntity()
    {
        return $this->targetEntity;
    }

    /**
     * @param int $targetEntity
     */
    public function setTargetEntity($targetEntity)
    {
        $this->targetEntity = $targetEntity;
    }

    /**
     * @return int
     */
    public function getPId()
    {
        return $this->pId;
    }

    /**
     * @param int $pId
     */
    public function setPId($pId)
    {
        $this->pId = $pId;
    }

    /**
     * @return int
     */
    public function getCreateTime()
    {
        return $this->createTime;
    }

    /**
     * @param int $createTime
     */
    public function setCreateTime($createTime)
    {
        $this->createTime = $createTime;
    }
}
```