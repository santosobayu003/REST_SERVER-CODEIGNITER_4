# REST_SERVER-CODEIGNITER_4
Restfull API menggunakan fitur CodeIgniter 4 
Download framework CodeIgniter 4 di https://codeigniter.com/download 

## Controller

```php
<?php

namespace App\Controllers;

use CodeIgniter\RESTful\ResourcePresenter;
use CodeIgniter\API\ResponseTrait;
use CodeIgniter\HTTP\Message;
use App\Models\Your_model;

class C_restServer extends ResourcePresenter
{

    use ResponseTrait;
    protected $Your_model;
    protected $format    = 'json';

    function __construct()
    {
        $this->Your_model = new Your_model();
    }

    public function read($id = null)
    {
        $id = $this->request->getVar('id');
        if ($id) {
            $data = $this->Your_model->select('*')->where('id_tbl', $id)->get()->getRowArray();
            if ($data) {
                return $this->respond($data);
            } else {
                return $this->failNotFound('id = ' . $id . ', Not found !');
            }
        } else {
            $data = $this->Your_model->findAll();
            if ($data) {
                return $this->respond($data);
            } else {
                return $this->failNotFound('Not found !');
            }
        }
    }

    public function create()
    {
        $field_1 = $this->request->getVar('field_1');
        $field_2 = $this->request->getVar('field_2');
        $data = [
            'field_1' => $field_1,
            'field_2' => $field_2
        ];

        $create = $this->Your_model->insert($data);

        if ($create) {
            return $this->respondCreated([
                'status' => true,
                'message' => 'Saved !',
            ]);
        } else {
            return $this->respond('Error 404 !');
        }
    }


    public function update_data()
    {
        $id = $this->request->getVar('id');
        $field_1 = $this->request->getVar('field_1');
        $field_2 = $this->request->getVar('field_2');
        $data = [
            'field_1' => $field_1,
            'field_1' => $field_2
        ];

        $update = $this->Your_model->update($id, $data);
        if ($update) {
            return $this->respondCreated([
                'status' => true,
                'message' => 'Update !',
            ]);
        } else {
            return $this->respond('Error 404 !');
        }
    }

    public function delete_data()
    {
        $id = $this->request->getVar('id');
        if ($id) {
            $delete = $this->Your_model->where('id_tbl', $id)->delete();
            if ($delete) {
                return $this->respondDeleted([
                    'status' => true,
                    'message' => 'Deleted !',
                ]);
            } else {
                return $this->respond('Error 404 !');
            }
        } else {
            return $this->failNotFound('Not found id !');
        }
    }
}

```
