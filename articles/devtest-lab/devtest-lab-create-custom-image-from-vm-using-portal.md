---
title: "Creación de una imagen personalizada de Azure DevTest Labs a partir de una máquina virtual | Microsoft Azure"
description: "Aprenda a crear una imagen personalizada en Azure DevTest Labs a partir de una máquina virtual aprovisionada mediante el portal de Azure."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Crear una imagen personalizada a partir de una máquina virtual

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Instrucciones paso a paso

Puede crear una imagen personalizada a partir de una máquina virtual aprovisionada y luego usar esa imagen personalizada para crear máquinas virtuales idénticas. Los siguientes pasos muestran cómo crear una imagen personalizada desde una máquina virtual:

1. Inicie sesión en el [Portal de Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Seleccione **Más servicios** y, luego, **DevTest Labs** en la lista.

1. En la lista de laboratorios, seleccione el laboratorio que desee.  

1. En la hoja del laboratorio, seleccione **Mis máquinas virtuales**.
 
1. En la hoja **Mis máquinas virtuales** , seleccione la máquina virtual desde la que quiere crear la imagen personalizada.

1. En la hoja de la máquina virtual, seleccione **Create custom image (VHD)**(Crear imagen personalizada [VHD]).

    ![Crear elemento de menú de imagen personalizada](./media/devtest-lab-create-template/create-custom-image.png)

1. En la hoja **Create image** (Crear imagen), escriba un nombre y una descripción para la imagen personalizada. Esta información se muestra en la lista de bases cuando se crea una máquina virtual.

    ![Crear imagen personalizada](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Seleccione si sysprep se ha ejecutado en la máquina virtual. Si no se ha ejecutado sysprep en la máquina virtual, especifique si quiere ejecutar sysprep cuando se cree una máquina virtual a partir de esta imagen personalizada.

1. Cuando termine de crear la imagen personalizada, seleccione **Aceptar** .

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Entradas blogs relacionadas

- [Custom images or formulas? (¿Imágenes personalizadas o fórmulas?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Copiar imágenes personalizadas entre instancias de Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Pasos siguientes

- [Agregar una máquina virtual al laboratorio](./devtest-lab-add-vm-with-artifacts.md)
